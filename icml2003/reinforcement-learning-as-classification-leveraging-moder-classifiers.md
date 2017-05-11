# Reinforcement Learning as Classification: Leveraging Modern Classifiers

Michail G. Lagoudakis, Ronald Parr

Duke

# Intro

增强式学习用于解决planning和control问题.

# Basic definitions and algorithms

重新回顾Markov Decision Process(MDP),policy iteration,approximate policy iteration,和rollouts.

## MDPs

deterministic policy $$\pi$$, 是状态空间S到行动空间A的映射.$$\pi(s)$$表示的是agent在状态s的时候采取的行动.$$V_\pi(s)$$是在policy $$\pi$$下,期望的reward,在状态s开始之后的总和.

$$V_\pi(s) = \mathbb{E}[\sum_t \gamma^t R(s_t,\pi(s_t)) | s_0 = s]$$

目标是希望能够找到找到最优的policy $$\pi^*$$.

## Policy Iteration

