# Tutorial: Deep Reinforcement Learning

Davud Sukter, Google DeepMind

# Reinforcement Learning

$$s$$：状态

$$a$$：行为

policy：决策，给定当前状态，决定当前行为，有两种

+ Deterministic policy：$$a = \pi (s)$$
+ Stochastic policy: $$a = \mathbb{P}(a|s)$$

value function：是将来行为／状态的reward

$$Q^{\pi}(s,a) = \mathbb{E}[r_{t+1} + \gamma r_{t+2} + \gamma^2 r_{t+3} + ... | s,a]$$

value function可以分解为Bellman equation:

$$Q^{\pi}(s,a) = \mathbb{E}_{s',a'} [ r + \gamma Q^{\pi}(s',a') |s,a ] $$

Bellman equation可以让我们很方便的使用DP方法来求解，因为这就是一个状态转移方程。可以通过穷尽搜索，找到所有状态，或者记忆化搜索，然后迭代地更新。

我们要选的就是optimal value function

$$Q^*(s,a) = \underset{\pi}{max} \, Q^{\pi}(s,a) = Q^{\pi^{*}}(s,a)$$