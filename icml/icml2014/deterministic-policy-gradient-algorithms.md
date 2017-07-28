# Deterministic Policy Gradient Algorithms

David Silver, Guy Lever, Nicolas Heess, Thomas Degris, Daan Wierstra, Martin Riedmiller

# Intro

这篇paper考虑deterministirc policy gradient，配合连续的动作空间。deterministic policy gradient有一个非常吸引人的特质：它是action-value function gradient的期望。并且展示了deterministic policy gradient比相对应的stochastic方法好很多。

当动作空间是连续的时候，RL通常使用policy gradient的方法。基本的想法是，使用一个概率分布来表示一个policy，$$\pi_\theta(a|s) = \mathbb{P}[a|s;\theta]$$，能够随机的根据state和参数，选取动作。