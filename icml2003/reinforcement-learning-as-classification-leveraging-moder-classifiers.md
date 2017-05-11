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

policy iteration是通过遍历序列$$\pi_1, \pi_2, ... , \pi_k$$，来找最优策略.优化通关计算Q-value，其定义是:

$$Q_{\pi_i}(s,a) = R(s,a) + \gamma \sum_{s'} P(s,a,s') V_{\pi_i}(s')$$

而优化过后的策略是:

$$\pi_{i+1} = arg\,max \, Q_{\pi_i}(s,a)$$

## Approximate methods

这类方法在状态空间很大的时候使用.一个常用的框架是approximate policy iteration(API).每一步都$$\pi_i$$不一定是exact answer,但是效果都不错.

# Choosing $$S_\rho$$

如何选择下一个状态,如果假设是均匀分布会有问题.最好是从当前policy $$\pi_i$$,得到一个状态的分布,而这个状态分布是通过训练器体现的.

因为分类器训练的样本是来自于$$\pi_i$$,当我们生成下一个状态$$\pi_{i+1}$$的时候,与当前状态没关系.可以通过设定一个step size $$\alpha$$ 来保证下一个迭代的policy与当前的policy差别不会太大.

$$\tilde \pi_{i+1} = \alpha \, arg\, max_a \, \hat Q_{\pi_i}(s,a) + (1-\alpha) \hat \pi_i $$

# Appendix
