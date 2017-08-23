# A Brief Survey of Deep Reinforcement Learning

Kai Arulkumaran, Marc Peter Deisenroth, Miles Brundage, Anil Anthony Bharath

# Intro

以往的RL受限于computation，只能处理low-dimension的数据。但如果引入了deep learning，就可以处理复杂的输入。

# Reward-Driven Behavior

RL的本质是通过interaction进行学习。一个RL agent会和环境进行交互，在收到一系列的observation之后，根据reward，改变自己的行为。

sequence action被reward终结。每一次进行state transition，都会得到一个scalar reward。agent的目标是为了学习到一个policy，能够最大化return。这里RL的agent有一个挑战是需要通过trial和error中学习最优的策略，并且state transition是未知的。

在RL中有这么几个挑战：

+ optimal policy需要通过和环境进行trial-and-error的尝试
+ observation包含了非常强的temporal correlation
+ agent需要处理long-range dependencies：一个序列的行为很可能在很长一段时间之后才会产生影响


# Reinforcement Learning Algorithm

有两个大类：基于value function和基于policy search。也有二者的结合，actor-critic。

## Value Functions

state-value $$V$$是expected return。

因为state transition未知，所以引入了state-action-value，或者也叫$$Q$$-function。这样方便使用遍历的方法将$$V$$和$$Q$$-函数联系起来。

## DP

引入了Bellman equation。

我认为这里介绍的不是特别好，划分有点混乱。将TD放在了DP里面介绍，和后面的sampling(MC)没有做很好的区分。所以这里补充几点：

1. bootstrapping，自助抽样法。在这里的作用是，每当我在时间t的时候，通过某些方法优化了policy，在t+1的时候，可以用这个优化过的policy接着进行预测。
2. TD方法，既包含了DP，也包含了抽样的想法。

## Sampling (MC)

Monte Carlo通过averaging the return from multiple rollouts of a policy的方法来估计expected return。

# Appendix

收录于IEEE Signal Processing Magazine