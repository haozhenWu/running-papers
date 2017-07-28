# Policy Gradient Methods for Reinforcement Learning with Function Approximation

Richard Sutton, David McAllester, Satinder Singh, Yishay Mansour

AT&T

# Intro

第一次将policy使用自己的函数估计值表示，与value function独立。并且展示了Williams的REINFORCE方法和actor-critic方法是这种方法的例子。

value function approximation有几个缺陷：
1. 找到的是deterministic policy
2. 对某一个action的value function只要有一点点小改变，就可能改变这个行为是否会被选中。比如Q-learning，Sarsa，DP对于很简单的MDP，都可能会收敛到不同的policy