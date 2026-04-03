---
title: actor-critic
categories:
  - 专业笔记
date: 2025-07-04 16:15:10
tags:
---

如今强化学习中很多成功的算法（PPO,SAC等）包含了将回报分解为value和advantage的思想。

这种分解为value和advantage的想法可以应用在on-policy和off-policy策略上。比如DDPG,就是分解为value和advantage的算法用在off-policy算法上。

Actor-Critic是一中混合方法，基于value-based Q-learning和policy-based method。

在Q-learning中，
$$
Q(s_t,a_t) = r_t + \gamma \max_a{Q_t(s_{t+1},a)}
$$

$$
\max_a{Q_t(s_{t+1},a)}
$$

不依赖于action a,因此我们可以
$$
Q(s_t,a_t) = r_t + \gamma V(s_{t+1})
$$
，V(s)只依赖于状态s.

其中这个V(s)叫做value-function.



现在我们可以将Q(s_{t},a_{t})分解为两部分：

- 一个是potential value V(s_{t})不依赖于a_{t}
- 另一部分是A(a_{t},s_{t})(也叫advantage) 取决于状态s_{t}的动作a_{t}

因此A(a_{t},s_{t})可以表示为：

$$
A(a_t, s_t) = r_t + \gamma V(s_{t+1}) - V(s_t)
$$


在这种方法下，我们产生value-function(可以通过神经网络估计出来)V(s)并且应用policy gradient对于advantage-function A(a,s)。我们应该产生2个function, value function和policy function,并且优化这2个fu nctions的参数。

通俗的解释，value function衡量不同state下的value,policy function被优化产生某状态下最合适的action.

在vanilla on-policy learning中，我们应用梯度下降(ascent) on
$$
E\left[\sum{\gamma r}\right]
$$
.当我们把梯度下降用在reduced A(a,s)而不是
$$
E\left[\sum{\gamma r}\right]
$$
，我们可以在复杂问题中得到更稳定的收敛。



在reward很大的时候，value loss比policy loss大很多。如果policy和value分开，各自训练中参数就会得到恰当的更新。(如果不分开，policy loss会因为很小被忽视，从而得不到充分的优化更新。)



你可以run Actor-Critic-based training通过batch或者non-batch

如果是每episodes作为一个batch进行优化，估计A_{t}可以是\sum\gamma r-V(s_{t})

如果是在episode中间，估计就是
$$
r_{t} + \gamma r_{t+1} + \cdots + \gamma^{T-1-t} r_{T-1} + \gamma^{T-t} V(s_T) - V(s_t)
$$
后者方法也叫temporal difference(TD) learning



Actor-Critic的想法和policy gradient相似。

但是在Actor-Critic,我们实用value function(ValueNet)来估计value(Q-value).

在此时的例子中，我们隔开policy network(action)和value network的权重参数。

但是，你也可以让policy和value共享network.在后者场景中，需要明确ratio.

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
        self.output = nn.Linear(hidden_dim,1)

    def forward(self, s):
        outs = self.hidden(s)
        outs = F.relu(outs)
        value = self.output(outs)
        return value

actor_func = ActorNet().to(device)
value_func = ValueNet().to(device)
gamma = 0.99

# pick up action with above distribution policy_pi
def pick_sample(s):
    with torch.no_grad():
        #   --> size : (1, 4)
        s_batch = np.expand_dims(s, axis=0)
        s_batch = torch.tensor(s_batch, dtype=torch.float).to(device)
        # Get logits from state
        #   --> size : (1, 2)
        logits = actor_func(s_batch)
        #   --> size : (2)
        logits = logits.squeeze(dim=0)
        # From logits to probabilities
        probs = F.softmax(logits, dim=-1)
        # Pick up action's sample
        a = torch.multinomial(probs, num_samples=1)
        # Return
        return a.tolist()[0]

env = gym.make("CartPole-v1")
reward_records = []
opt1 = torch.optim.AdamW(value_func.parameters(), lr=0.001)
opt2 = torch.optim.AdamW(actor_func.parameters(), lr=0.001)
for i in range(1500):
    #
    # Run episode till done
    #
    done = False
    states = []
    actions = []
    rewards = []
    s, _ = env.reset()
    while not done:
        states.append(s.tolist())
        a = pick_sample(s)
        s, r, term, trunc, _ = env.step(a)
        done = term or trunc
        actions.append(a)
        rewards.append(r)

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

    # Optimize value loss (Critic)
    opt1.zero_grad()
    states = torch.tensor(states, dtype=torch.float).to(device)
    cum_rewards = torch.tensor(cum_rewards, dtype=torch.float).to(device)
    values = value_func(states)
    values = values.squeeze(dim=1)
    vf_loss = F.mse_loss(
        values,
        cum_rewards,
        reduction="none")
    vf_loss.sum().backward()
    opt1.step()

    # Optimize policy loss (Actor)
    with torch.no_grad():
        values = value_func(states)
    opt2.zero_grad()
    actions = torch.tensor(actions, dtype=torch.int64).to(device)
    advantages = cum_rewards - values
    logits = actor_func(states)
    log_probs = -F.cross_entropy(logits, actions, reduction="none")
    pi_loss = -log_probs * advantages
    pi_loss.sum().backward()
    opt2.step()

    # Output total rewards in episode (max 500)
    print("Run episode{} with rewards {}".format(i, sum(rewards)), end="\r")
    reward_records.append(sum(rewards))

    # stop if reward mean > 475.0
    if np.average(reward_records[-50:]) > 475.0:
        break

print("\nDone")
env.close()
```
