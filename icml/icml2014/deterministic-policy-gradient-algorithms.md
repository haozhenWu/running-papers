# Deterministic Policy Gradient Algorithms

David Silver, Guy Lever, Nicolas Heess, Thomas Degris, Daan Wierstra, Martin Riedmiller

# Intro

这篇paper考虑deterministirc policy gradient，配合连续的动作空间。deterministic policy gradient有一个非常吸引人的特质：它是action-value function gradient的期望。并且展示了deterministic policy gradient比相对应的stochastic方法好很多。