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

Monte Carlo通过averaging the return from multiple rollouts of a policy的方法来估计expected return。也就是sample多条路径，然后取平均。

最开始的MC只能解决episodic problem。而且因为是从start time step，通过sampling，直接走到底，或者说直接走到end time step，所以不存在通过t时刻的policy improvement，来帮助优化t之后时刻的policy。从而不存在bootstrapping。

$$TD(\lambda)$$就是1-step TD和纯MC之间的缓冲带。

另外一个主要的value-based方法是使用advantage function $$A(s,a)$$，而不是$$Q$$-function。advantage表示的是采取某个action比某个baseline要好，也就是一种relative value function。

$$A = V - Q$$

## Policy Search

直接搜索最优的policy，不需要通过value function。将policy进行参数化 $$\pi_\theta$$，然后目标是为了最大化 $$E[R|\theta]$$。

对于连续的特征，可以用gaussian distribution，而参数是mean和deviation。对于离散的特征，那么就是相互独立的。结果就是能够产生一个stochastic policy，从而能够sample actions。

如果使用gradient-free方法，那么找到更好的policy就需要一些heuristic search，比如hill-climbing。

## Policy Gradient

紧接着上面，如果使用基于gradient的方法，为了得到$$argmax_{\pi} \, E[R|\pi_\theta]$$，要么使用deterministic policy，要么使用stochastic policy配合sampling。

对于更一般的stochastic情况，使用MC来估计expected return。这会带来一个问题：有随机分布产生的action sample，相互之间的gradient无法传递。因此引入了REINFORCE rule。核心想法就是我们对gradient进行估计，一个被函数$$f$$（可以是很多，比如reward，state-action value，state value等等）加权的log-likelihood。

## Actor-critic Method

将value-function和policy representation结合起来。

使用一个actor(policy)，从critic(value function)中得到反馈，从而优化策略。而actor critic使用value function作为policy gradient的baseline。

[智能单元](https://zhuanlan.zhihu.com/p/26882898)的解释非常到位。再按照我的理解说一下：如果我们有了某种oracle，知道reward，不需要进行计算，那么就变成了一个仅仅依靠gradient来优化policy的算法，这就是gradient policy。但是，再更多的情况下，我们并不知道真实的reward，就要用各种方法来模拟$$Q$$-value，那么这就是Actor Critic。Critic (value network)预测的值会指导Actor (policy network)的更新。

# Value Functions

value function认为是隐藏于environment下的一种表达方式。现在很多方法都是使用DL来讲state作为输入，从而输出某种value function。

## DQN

DQN就是将image或者frame作为输入，来预测输出。很多Atari游戏都可以使用这种方法。

# Appendix

收录于IEEE Signal Processing Magazine