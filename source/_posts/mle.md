---
title: mle
categories:
  - 专业笔记
date: 2026-03-21 14:43:21
tags:
---



#### 极大似然估计(MLE)

寻找一组参数，使得这组参数下观测到当前数据的概率最大。

如果数据是完整的即没有隐藏变量，所有特征和标签都已知），MLE 的求解通常是一个直接的优化问题。

### 核心流程：四步走策略

假设我们有一组独立同分布（i.i.d.）的数据 $$$X = \{x_1, x_2, \dots, x_n\}$$$，我们要估计模型参数 $\theta$。

#### 第一步：构建似然函数 (Likelihood Function)

似然函数 $L(\theta)$ 表示在参数 $\theta$ 下，观测到整组数据 $X$ 的联合概率。由于数据是独立的，联合概率等于各样本概率的乘积：

$$L(\theta) = P(x_1, x_2, \dots, x_n | \theta) = \prod_{i=1}^{n} P(x_i | \theta)$$

#### 第二步：取对数似然 (Log-Likelihood)

由于连乘运算在求导时非常麻烦，且容易导致数值下溢（很多概率相乘会变得极小），我们通常对 $L(\theta)$ 取自然对数。由于 $\ln(x)$ 是单调递增函数，使 $\ln L(\theta)$ 最大的 $\theta$ 同样会使 $L(\theta)$ 最大：

$$\ell(\theta) = \ln L(\theta) = \sum_{i=1}^{n} \ln P(x_i | \theta)$$

这样，**乘法变加法**，大大简化了计算。

#### 第三步：求导并令其为零

为了找到极值点，我们对参数 $\theta$ 求偏导（即计算梯度），并令导数为 0：

$$\frac{\partial \ell(\theta)}{\partial \theta} = 0$$

#### 第四步：求解参数

解上述方程，得到的解 $\hat{\theta}_{MLE}$ 即为极大似然估计值。

------

### 2. 经典案例：高斯分布的参数估计

在机器人动作建模中，我们经常假设动作噪声服从高斯分布 $N(\mu, \sigma^2)$。如果我们有 $n$ 个位置观测值，如何求均值 $\mu$？

1. **写出单个样本的概率密度**：$P(x_i|\mu, \sigma) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x_i-\mu)^2}{2\sigma^2}}$

2. **写出对数似然**：

   $$\ell(\mu, \sigma) = \sum_{i=1}^n \left( -\ln(\sqrt{2\pi}\sigma) - \frac{(x_i-\mu)^2}{2\sigma^2} \right)$$

3. **对 $\mu$ 求导**：

   $$\frac{\partial \ell}{\partial \mu} = \sum_{i=1}^n \frac{x_i - \mu}{\sigma^2} = 0$$

4. **得出结果**：

   $$\hat{\mu} = \frac{1}{n} \sum_{i=1}^n x_i$$

   这证明了在正态分布假设下，**算术平均值**就是最科学的极大似然估计。
