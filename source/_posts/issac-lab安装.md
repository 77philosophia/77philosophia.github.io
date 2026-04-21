---
title: issac-lab安装
date: 2026-04-09 20:10:37
tags:
  - issac lab
category:
  专业笔记
---

1. 确认环境
    硬件： ubuntu20.04, A6000

安装：主要参考https://docs.robotsfan.com/isaaclab/source/setup/installation/pip_installation.html
 比较顺利，没有遇到什么麻烦。

```
#虚拟环境的Python版本必须与Isaac Sim的Python版本匹配。
#- 对于Isaac Sim 5.X，所需的Python版本为3.11。
#- 对于Isaac Sim 4.X，所需的Python版本为3.10。
#1.conda环境安装issaclab  
conda create -n env_isaaclab python=3.11
conda activate env_isaaclab

#2.确保pip最新
pip install --upgrade pip

#3. 安装issac sim
pip install "isaacsim[all,extscache]==5.1.0" --extra-index-url https://pypi.nvidia.com

#4.安装pytorch相关包
pip install -U torch==2.7.0 torchvision==0.22.0 --index-url https://download.pytorch.org/whl/cu128

#5.验证安装
isaacsim
```

验证安装成功。

![image-20260409202250382](issac-lab%E5%AE%89%E8%A3%85/image-20260409202250382.png)

