---
title: ubuntu-nvidia-driver-noNetwork
categories:
  - 专业笔记
date: 2025-04-28 17:56:37
tags:
---

# ubuntu安装Nvidia显卡驱动之后没有network

https://blog.csdn.net/weixin_44286126/article/details/131455726



排查思路：

1.ubuntu会保留旧的镜像，可以在引导模式(出现图标esc)切换到旧镜像，保持网络连接。然后dpkg 查看新旧镜像是否有安装包的差异。

2. 原因是安装ubuntu显卡驱动的时候把内核部分包也升级了。
