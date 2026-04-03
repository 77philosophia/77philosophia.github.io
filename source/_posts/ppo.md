---
title: ppo
categories:
  - 专业笔记
date: 2025-07-04 16:15:53
tags:
---

PPO 是强化学习中一种基于策略梯度（Policy Gradient）的算法，旨在解决传统策略梯度方法的两个核心问题：

1. **训练不稳定性**
   - 策略梯度方法（如REINFORCE、Actor-Critic）直接更新策略时，若步长（学习率）过大，可能导致策略突然崩溃（性能骤降），难以恢复。
   - 例如，在更新后，策略可能过度偏向某些动作，导致探索不足或陷入局部最优。
2. **样本效率低**
   - 传统方法需要大量与环境交互的样本才能收敛，而PPO通过限制策略更新的幅度，使得同一批数据可以多次复用（即“经验回放”的策略版本），提升数据利用率。







Proximal Policy Optimization(PPO)是如今强化学习中最成功的算法之一。

PPO是一种优化随机策略的on-policy的方法。为了避免效果损失，PPO阻止更新stepping过大。

为了避免过大的更新，PPO有两种变体，PPO-Penalty和PPO-Clip.

下面主要介绍PPO-Penalty.

PPO-Clip比PPO-Penalty更简单。PPO-Clip通过clip \epsilon



在Actor-Critic方法中，算法会学习策略参数\theta通过advantages A。

当我们选择action a有很大的优势A的时候， P(a|\pi_{\theta}(s))的增长会比P(a|\pi_{\theta_{old}}(s))大很多。所以对于参数\theta也会有下面的优化策略：

$$
\max_{\theta} E \left[ \frac{P(a | \pi_\theta (s))}{P(a | \pi_{\theta_{old}} (s))} A \right]
$$
为了避免很大的policy updates的更新，PPO penaltizes 会类似下面：

$$
\max_{\theta} E \left[ \frac{P(a | \pi_\theta (s))}{P(a | \pi_{\theta_{old}} (s))} A - \beta \cdot \verb|penalty| \right]
$$


在PPO-penalty中，KL散度用来估计这个惩罚项。

$$
\verb|penalty| := \verb|KL| \left( P(\cdot | \pi_{\theta_{old}} (s)) \| P(\cdot | \pi_\theta (s)) \right)
$$


附：KL散度

我们假设P(x)和Q(x)都是随机分布，KL散度被定义为：

$$
\verb|KL|( P \| Q ) := -\int{P(x) \ln{\frac{Q(x)}{P(X)}}}dx
$$
（需要最大化KL散度的时候就在前面加负号）

这个定义下：\verb|KL|( P \| Q )总是正值或者0，是0的时候有且只有两个分布是一样的时候。

KL散度衡量两个分布的差异，如果Q距离P很远，那么KL散度会相当大。

KL散度具体可以参考[chapter1.6](https://www.microsoft.com/en-us/research/wp-content/uploads/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)

注意，KL散度不是对称的，也就是说
$$
\verb|KL|( P \| Q ) \neq \verb|KL|( Q \| P )
$$
，并且
$$
arg\,min_Q \verb|KL|( P \| Q ) \neq arg\,min_Q \verb|KL|( Q \| P )
$$
回到我们的场景，为了惩罚P(\cdot | \pi_{\theta_{old}} (s))和P(\cdot | \pi_\theta (s))更新差异过大，我们可以

$$
\max_{\theta} E \left[ \frac{P(a | \pi_\theta (s))}{P(a | \pi_{\theta_{old}} (s))} A - \beta \cdot \verb|KL| \left( P(\cdot | \pi_{\theta_{old}} (s)) \| P(\cdot | \pi_\theta (s)) \right) \right] \;\;\;\;\
$$
; (1)

这样即使第一项
$$
\frac{P(a | \pi_\theta (s))}{P(a | \pi_{\theta_{old}} (s))}
$$
很大，\theta也会因为惩罚项的存在避免过大的更新。

最后我们最大化等式（1），通过最小化value loss来优化value值估计。



依旧还是Actor-Critic网络：

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

class ActorNet(nn.Module):
    def __init__(self, hidden_dim=16):
        super().__init__()

        self.hidden = nn.Linear(4, hidden_dim)
        self.output = nn.Linear(hidden_dim, 2)

    def forward(self, s):
        outs = self.hidden(s)
        outs = F.relu(outs)
        logits = self.output(outs)
        return logits

class ValueNet(nn.Module):
    def __init__(self, hidden_dim=16):
        super().__init__()

        self.hidden = nn.Linear(4, hidden_dim)
        self.output = nn.Linear(hidden_dim, 1)

    def forward(self, s):
        outs = self.hidden(s)
        outs = F.relu(outs)
        value = self.output(outs)
        return value

actor_func = ActorNet().to(device)
value_func = ValueNet().to(device)
```



不分别最大化
$$
\frac{P(a | \theta_{new})}{P(a | \theta_{old})} A - \beta \cdot \verb|KL| \left( P(\theta_{old}) \| P(\theta_{new}) \right)
$$
和最小化value loss L，我们简化为下面的式子：

$$
(-1) \frac{P(a | \theta_{new})}{P(a | \theta_{old})} A + \beta \cdot \verb|KL| \left( P(\theta_{old}) \| P(\theta_{new}) \right) + L
$$
这里，P和Q的分布都是离散的，因此KL散度为：

$$
\verb|KL| \left( P(\theta_{old}) \| P(\theta_{new}) \right) = -\sum_a \left( P(a | \theta_{old}) \ln{\frac{P(a | \theta_{new})}{P(a | \theta_{old})}} \right)
$$
不像之前policy和value分开训练，这次我们用一个loss radio来控制二者的比例。

在分类分布中,log probability和cross-entropy损失的负值相等。所以可以用-torch.nn.functional.cross_entropy()得到log probability值。

为了训练平稳，advatange可以进行归一化：
$$
(A-\mu) / \sigma
$$
