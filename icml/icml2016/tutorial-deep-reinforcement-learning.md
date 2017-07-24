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

