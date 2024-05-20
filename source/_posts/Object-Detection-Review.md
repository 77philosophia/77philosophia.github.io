---
title: Object-Detection-Review
date: 2019-12-17 22:35:15
tags:
---

# 论文题目：Objection Detection with Deep Learning: A Review

##### 1.Introduction

 目标检测子类包括脸部检测(face detection)，行人检测(pedestrian detection)，躯体骨骼检测(skeleton detection)等等。

 目标检测的困难：在视角(viewpoints),姿势(poses),遮挡(occlusions),和光照条件(lightling conditions)存在很大不同。

 目标检测的任务**=目标定位(object location)+目标分类(object classification)**。因此pipeline有三个阶段：**目标区域选取(informative region selection),特征提取(feature extraction),分类(classification)**

###### a.Informative region selection

 解决方法是multi-scale sliding window。因为你不知道目标会在图像的哪个位置出现，有多大。这个方法的缺点显而易见：expensive

###### b.feature extraction

 sift,hog算子等都可以用来提取物体特征，但是由于表面，光照，北京等影响很难设计一个具体的算子去特别识别一个目标。

###### c.classification

 比如SVM，AdaBoost, DPM

以上描述的传统算法缺点1、sliding window低效性。2、semantic gap cannot be bridged by the low-level descriptors.我们需要更为强大的特征提取算子。

