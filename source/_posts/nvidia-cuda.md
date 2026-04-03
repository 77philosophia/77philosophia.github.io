---
title: nvidia-cuda
categories:
  - 专业笔记
date: 2025-05-07 11:25:48
tags:
---

# nvcc和Nvidia-smi

### **1. `nvidia-smi`（NVIDIA System Management Interface）**

- **作用**：监控和管理 NVIDIA GPU 设备。

- **功能**：

  - 查看 GPU 的实时状态（利用率、温度、功耗等）。
  - 显示已安装的 **GPU 驱动版本**。
  - 管理 GPU 进程（如杀死占用 GPU 的进程）。

- **使用场景**：

  - 检查 GPU 是否被识别。
  - 监控深度学习训练时的 GPU 负载。

- **示例输出**：

  bash

  ```
  $ nvidia-smi
  +-----------------------------------------------------------------------------+
  | NVIDIA-SMI 535.161.07   Driver Version: 535.161.07   CUDA Version: 12.2     |
  |-------------------------------+----------------------+----------------------+
  | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
  | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
  |===============================+======================+======================|
  |   0  NVIDIA RTX 4090     On   | 00000000:01:00.0 Off |                  Off |
  |  0%   45C    P8    15W / 450W |      0MiB / 24576MiB |      0%      Default |
  +-------------------------------+----------------------+----------------------+
  ```

  - 这里显示的 `CUDA Version: 12.2` 是 **驱动支持的最高 CUDA 版本**，但实际安装的 CUDA 工具包版本可能不同。

### **2. `nvcc`（NVIDIA CUDA Compiler）**

- **作用**：CUDA 工具包中的编译器，用于编译 CUDA C/C++ 代码。

- **功能**：

  - 编译和优化 GPU 加速的代码。
  - 显示当前安装的 **CUDA 工具包版本**。

- **使用场景**：

  - 开发 CUDA 程序时编译代码。
  - 验证 CUDA 环境是否配置正确。

- **示例输出**：

  bash

  ```
  $ nvcc --version
  nvcc: NVIDIA (R) Cuda compiler
  release 12.3, V12.3.107
  Build cuda_12.3.r12.3/compiler.33957004_0
  ```

  - 这里显示的 `release 12.3` 是 **实际安装的 CUDA 工具包版本**。



## 实际经验

1. 拿到一台服务器，先根据显卡型号安装最高版本的cuda driver，cuda driver里面会有一个cuda version,nvidia-smi显示的时候在右上角，是支持的最高版本的cuda toolkit.实际运行的时候决定的。此时还不代表什么runtime的实际含义。
2. 安装cuda toolkit https://docs.ksyun.com/documents/2591
   1. 进入英伟达官网，选择对应的cuda版本（比如12.4, 11.8之类的），下载runfile(local)，进行安装（注意不要重复选择安装已经安装的cuda driver）,只安装cuda toolkit即可。
   2. 在zshrc中修改路径(注意新安装放在$PATH的前面还是后面)
   3. nvcc --version验证此时的cuda toolkit



