---
title: comfyui环境搭建+问题记录
categories:
  - 专业笔记
date: 2025-01-07 19:15:27
tags:
---

### 环境安装

1. import torchaudio的时候报错“undefined symbol”

   ![image-20250107191646057](comfyui-env/image-20250107191646057.png)

推测还是版本不兼容，按照下面这个博客的版本进行安装

**pip install torch==2.1.0**

**pip install torchvision==****0.16.0**

**pip install torchaudio==****2.1.0**

**https://blog.csdn.net/qq_45599476/article/details/140207176**

再次启动就好了

![image-20250107191710761](comfyui-env/image-20250107191710761.png)



2. 在点击Queue Prompt出图的时候报错

   ![image-20250107191754873](comfyui-env/image-20250107191754873.png)

现这个工程中**xformers**也没有成功运行。

**https://github.com/comfyanonymous/ComfyUI/issues/4870**

**The torch repo has a release wheel of xformers that works with torch 2.4.1**

**pip3 install -U xformers --index-url** **https://download.pytorch.org/whl/cu124** **should pull**

***0.0.28.post1\***

**https://pytorch.org/get-started/previous-versions/**

解决还是变包：

**pip install torch==2.4.1**

**Pip install torchvision==0.19.1**

**Pip install torchaudio==2.4.1**

**pip3 install -U xformers --index-url** **https://download.pytorch.org/whl/cu124** **should pull**

***0.0.28.post1\***

成功出图

![image-20250107191854078](comfyui-env/image-20250107191854078.png)

![image-20250107191903222](comfyui-env/image-20250107191903222.png)



### 测试：

安装和sd跑同一个prompt

![image-20250107191927650](comfyui-env/image-20250107191927650.png)

![image-20250107191936508](comfyui-env/image-20250107191936508.png)
