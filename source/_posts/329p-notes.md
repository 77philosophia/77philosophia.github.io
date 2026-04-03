---
title: 329p-notes
categories:
  - 专业笔记
date: 2026-03-09 10:12:15
tags:
---

### 1-data I

lecture没有什么内容，推荐了两篇论文：

补充材料：Challenges in Deploying Machine Learning: a Survey of Case Studies（https://arxiv.org/pdf/2011.09926）

- 1.数据获取方面的困难
  - 要从多个独立的数据源cooperate数据
  - 缺乏高方差的数据（比如强化学习的agent没有单独的探索策略），现在合成数据可能是一个解决思路。
  - tips: ui终端用户的设计是影响数据收集的关键
- 2.数据分析的困难：拿到新数据如何分析它
  - 可视化
- 3.模型学习：
  - tips:小模型的运用在实际部署中有优势：比如浅层神经网络、pca，随机森林，决策树。
    - 举例在做异常检测没有用什么dl，就是简单的阈值和pca
    - 用决策树分析银行if-then预测流失-》 for 可解释性
  - difficulty: hbo 超参
  - 数据漂移：新的数据输入fresh旧的预测
- 4.一个发现：
   -机器学习面对的具体应用往往比较具体，也就是说需要工程师去分析哪一条数据，哪个过程的推理，因此开箱即用的机器学习工具反而不好做。
  5. ci/cd的部署过程可能也不好做。

https://arxiv.org/pdf/1811.03402 A Survey on Data Collection for Machine Learning
 信息增益不大，略过。



### 2.data-II



