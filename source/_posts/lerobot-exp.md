---
title: lerobot-exp
categories:
  - 专业笔记
date: 2025-12-22 14:07:27
tags: lerobot
---

###  购买和组装

淘宝购买的“幻尔”开源六轴机械臂，买的成品，和网上的开源方案比，不用进行3D打印和电路级别的组装，拿到之后只用安装腕部相机和全景相机，进行机械臂的校准即可。

接下来记录几个关键的步骤。



### usb端口绑定

> [!IMPORTANT]
>
> 我有一个LEROBOT主臂主板USB口是/dev/ttyACM0，还有一个从臂usb口是/dev/ttyACM1,我想进行端口绑定避免每次插拔都要重新CHMOD 666

这种情况在机器人开发（如 LeRobot）中非常常见。由于 Linux 系统分配 `/dev/ttyACM*` 是按照插拔顺序随机分配的，且默认权限受限，最稳妥的解决方案是**编写 udev 规则**。

通过 udev 规则，你可以根据硬件的唯一序列号（Serial Number）将它们永久映射为固定名称（如 `/dev/master_arm` 和 `/dev/slave_arm`），并自动设置 `0666` 权限。

------

### 第一步：获取设备的硬件信息

你需要找到两个主板的 **idVendor**、**idProduct** 以及最重要的 **serial**（序列号）。

1. 拔掉从臂，只插上**主臂**。

2. 运行以下命令查看信息：

   Bash

   

   ```
   udevadm info -a -n /dev/ttyACM0 | grep -E "idVendor|idProduct|serial" | head -n 3
   ```

   *注意：如果你的主臂在* `/dev/ttyACM1`*，请相应修改路径。*

3. **记录输出结果**，例如：

   - `ATTRS{idVendor}=="2e8a"`
   - `ATTRS{idProduct}=="000a"`
   - `ATTRS{serial}=="1234567890AB" `（这是区分两个相同型号主板的关键）

4. 拔掉主臂，插上**从臂**，重复上述步骤，记录从臂的序列号。

------

### 第二步：创建 udev 规则文件

创建一个新的规则文件：

Bash



```
sudo nano /etc/udev/rules.d/99-lerobot.rules
```

在文件中输入以下内容（请将 `xxxx` 替换为你刚才查到的实际数值）：

Bash



```
# 主臂 (Master Arm)
KERNEL=="ttyACM*", ATTRS{idVendor}=="xxxx", ATTRS{idProduct}=="xxxx", ATTRS{serial}=="主臂序列号", MODE="0666", SYMLINK+="master_arm"

# 从臂 (Slave Arm)
KERNEL=="ttyACM*", ATTRS{idVendor}=="xxxx", ATTRS{idProduct}=="xxxx", ATTRS{serial}=="从臂序列号", MODE="0666", SYMLINK+="slave_arm"
```

------

### 第三步：应用规则并验证

1. **加载新规则：**

   Bash

   

   ```
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

2. **验证绑定是否成功：** 插上两个主板，运行：

   Bash

   

   ```
   ls -l /dev/master_arm /dev/slave_arm
   ```

   你会看到类似这样的输出，显示它们已经指向了 `ttyACM*`： `lrwxrwxrwx 1 root root 7 Dec 18 10:00 /dev/master_arm -> ttyACM0`

3. **检查权限：**

Bash



```
ls -l /dev/ttyACM*
```

你会发现权限已经是 `crw-rw-rw-` (即 666)，不再需要手动修改。

------

### 核心优势

- **持久性：** 无论重启还是重新插拔，`/dev/master_arm` 永远指向正确的主板。
- **零配置：** 你在 LeRobot 的代码或配置文件中，直接将端口设置为 `/dev/master_arm` 即可。
- **权限自动化：** `MODE="0666"` 彻底解决了权限拒绝（Permission Denied）的问题。







