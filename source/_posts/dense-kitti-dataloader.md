---
title: dense-kitti-dataloader
date: 2019-08-06 23:17:10
tags:
---

## dense kitti dataloader

这篇文章介绍kitti的稀疏深度图生成dense depth map以及读出相应的dataloader，深度图的colormap可视化。主要用于深度估计。

### kitti to dense

### 准备

 KITTI的深度数据是sparse depth map，可以从官网下载到深度数据。[original depth map](http://www.cvlibs.net/datasets/kitti/eval_depth_all.php)

 KITTI的RGB数据用的是image_02。可以从官网下载到。[Raw data](http://www.cvlibs.net/datasets/kitti/raw_data.php).这个可能要找代理服务器下载，我在网上看到也有人把数据迁移到国内服务器的。

 由sparse depth map生成dense depth map的算法采用的是NYU的toolkit。[深度补全函数](https://github.com/jjhartmann/Toolbox-NYU-Depth-V2/blob/master/fill_depth_colorization.m)

### 过程

 新建一个文件夹，将深度解压的文件夹 data_depth_annotated ，原RGB数据RGB Raw的文件夹 raw_data_downloader，还有存放深度数据的文件夹 data_depth_to_dense放到一个目录下。说明：data_depth_to_dense 我就是把data_depth_annotated拷贝过来了，然后把文件名由data_depth_annotated改为data_depth_to_dense.因为反正图片数据要重写的，我拷贝的目的是不用重新建存放图片的目录。

 准备三个文件夹下每张图片的路径存为三个csv文件。（写一个python脚本）

 运行主程序 main.m。

### 可视化

可视化的代码：

```
import cv2
im =cv2.imread('image/path')
im_color = cv2.applyColorMap(im,cv2.COLORMAP_JET)
```

来自这个链接：[可视化](https://github.com/QianshengGu/KITTI_Dense_Depth.git)

但是这个链接的kitti处理存放数据的时候采用最大最小值归一化，而不是存储的真实数据。一方面可能导致结果不太精确，另一方面导致ground truth的深度值有零值存在，在输入深度网络训练的时候采用RMSE_log的损失的时候会导致损失函数无穷大。

### kitti dataloader

 这个dataloader与我上面说的文件路径是一致的。

参考github我之后会放上去。

https://github.com/77philosophia/kitti_data_depth_loader
