# Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks

Chelsea Finn, Pieter Abbeel, Sergey Levine

# Intro

通过学习一个meta-learner，在few-shot learning的设定下，对于inefficient data 或者 新加入的task 能够进行快速的训练。类似进行了一个热启动。

# Model-Agnostic Meta-Learning

few-shot meta-learning的目的是能够快速的adapt到新的task，而且只依靠很少的训练数据和迭代次数。

few-shot learning中，假设T个任务符合某种分别$$p(\tau)$$。

为了能够达到快速的transfer learning，会侧重于找到general-purpose representation。会找到对于task changes非常敏感的模型参数。也就是原来loss gradient更新相比，参数中很小的改变会引起$$p(\tau)$$中的task性能的极大提升。

提出了一个meta-objective

$$min_\theta \sum_{\tau_i} \mathcal{L}_{\tau_i} ( f_{\theta_i'} ) = \sum_{\tau_i} \mathcal{L}_{\tau_i} (f_\theta - \alpha \bigtriangledown_\theta \mathcal{L}_{\tau_i} (f_\theta)) $$

# Species of MAML

讨论了meta-learning的几种变形，在监督式学习和增强式学习上。

## 监督式问题

在这里主要还是在一个task上通过少量数据学习到了一些有用信息。而且也是包含了一个假设条件，就是task之间有类似的信息。

使用$$p(\tau)$$产生K个iid的观察，然后task loss就是使用这个observation得到的loss。s