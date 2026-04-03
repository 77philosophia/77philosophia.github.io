---
title: policy-gradient(1999)
categories:
  - 专业笔记
date: 2025-07-04 16:14:26
tags:
---

q-learning的网络是估计q-value，然后下个action选择当前state下q-value最大的那个action.

policy-gradient是建立一个网络，帮助选择当前state下面采样action.



Q-learning算法的缺点：无法处理连续空间、经验贪婪搜索等。

为了克服这些问题，一部分的算法引入了随机分布。

连续action空间的任务在RL中依然很具有挑战性，可以参考https://tsmatz.wordpress.com/2021/11/11/reinforcement-learning-visual-attention-in-minecraft/

之前的算法下个action的选择基于Q-table或者Q-function,算法的policy是确定性的。

本算法下一个动作是基于随机空间(就是)，策略将被训练为**输出随机分布的参数**（由随机空间决定），而非直接选择动作本身。

比如：

- 对于离散动作空间，可以使用**分类分布（Categorical Distribution）**；
- 对于连续动作空间（如Box类型），可以使用**高斯分布（Gaussian Distribution）**；
- 对于有界连续动作空间，则可以使用**Beta分布**。

通过引入随机空间，您不再需要应用经验贪婪探索。



在policy gradient中，action a是从依靠policy \pi的分布P中挑选的。

即动作分布可以表示为P(a|\pi_\theta(s)),\pi_\theta(s)是policy，\theta是这个policy的参数。如果当前action a已知，action取决于policy \pi. (比如分类分布、高斯分布)。

```
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

class PolicyPi(nn.Module):
    def __init__(self, hidden_dim=64):
        super().__init__()

        self.hidden = nn.Linear(4, hidden_dim)
        self.classify = nn.Linear(hidden_dim, 2)

    def forward(self, s):
        outs = self.hidden(s)
        outs = F.relu(outs)
        logits = self.classify(outs)
        return logits

policy_pi = PolicyPi().to(device)
```



在CarPole的例子中，我们假设P是一个分类分布(因为action space是离散的)，\pi_\theta(s)是一个全连接网络，有2维输出。输出是logits value。

当logits l_0,l_1给出时，概率是\frac{e^{l_0}}{e^{l_0}+e^{l_1}}，\frac{e^{l_1}}{e^{l_0}+e^{l_1}}

```
gamma = 0.99

# Pick up action with above distribution policy_pi
def pick_sample(s):
    with torch.no_grad():
        #   --> size : (1, 4)
        s_batch = np.expand_dims(s, axis=0)
        s_batch = torch.tensor(s_batch, dtype=torch.float).to(device)
        # Get logits from state
        #   --> size : (1, 2)
        logits = policy_pi(s_batch)
        #   --> size : (2)
        logits = logits.squeeze(dim=0)
        # From logits to probabilities
        probs = F.softmax(logits, dim=-1)
        # Pick up action's sample
        a = torch.multinomial(probs, num_samples=1)
        # Return
        return a.tolist()[0]
```



考虑下面的expectationE{\pi_\theta}[R]:

E_{\pi_\theta}[R]=\int_a{R(a) \cdot P(a|\pi_\theta(s))}

R是cumulative exptected rewards: R=\sum\gamma r

为了最大化E的到最优的\theta,我们应用梯度下降：\theta_{t+1} \leftarrow \theta_t + \Delta \theta

其中：

\Delta \theta = \alpha R \cdot \nabla_{\theta} \log{P(a|\pi_\theta(s))} \;\;\;\;\; (1)

\alpha是learning rate.

上面的公式来自于下面的公式，直接知道\nabla E很难，但是下面这个可以在每一步的梯度算法里计算：\nabla_{\theta} \int{R(a) \cdot P(a|\pi_\theta(s))} = \int{R(a) \nabla_{\theta}{\log P(a|\pi_\theta(s))} \cdot P(a|\pi_\theta(s))}



补充：

原始公式：

\nabla_{\theta} \int{R(a) \cdot P(a|\pi_\theta(s))} = \int{R(a) \nabla_{\theta}{\log P(a|\pi_\theta(s))} \cdot P(a|\pi_\theta(s))}

更新公式：

\Delta \theta = \alpha R \cdot \nabla_{\theta} \log{P(a|\pi_\theta(s))} \;\;\;\;\; (1)

实际上是用**单次采样**的蒙特卡洛估计代替了期望。

在公式1中，如果excepted reward R是正向大，调整\theta在状态s采取action a的概率会增加。响应的，如果expected reward R是负数，\theta会被调整去降低这个概率。

```
    #
    # Get cumulative rewards
    #
    cum_rewards = np.zeros_like(rewards)
    reward_len = len(rewards)
    for j in reversed(range(reward_len)):
        cum_rewards[j] = rewards[j] + (cum_rewards[j+1]*gamma if j+1 < reward_len else 0)

    #
    # Train (optimize parameters)
    #
    states = torch.tensor(states, dtype=torch.float).to(device)
    actions = torch.tensor(actions, dtype=torch.int64).to(device)
    cum_rewards = torch.tensor(cum_rewards, dtype=torch.float).to(device)
    opt.zero_grad()
    logits = policy_pi(states)
    # Calculate negative log probability (-log P) as loss.
    # Cross-entropy loss is -log P in categorical distribution. (see above)
    log_probs = -F.cross_entropy(logits, actions, reduction="none")
    loss = -log_probs * cum_rewards
    loss.sum().backward()
    opt.step()
```



你可以发现，该算法通过采样action's policy P( \cdot | \pi_\theta (s))来提高随机策略的效果，不仅依赖于Q-learning和DQN算法中的empirical greedy action sampling。这种方式也叫on-policy learning.
