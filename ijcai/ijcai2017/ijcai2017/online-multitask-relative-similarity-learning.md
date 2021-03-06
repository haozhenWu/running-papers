# Online Multitask Relative Similarity Learning

Shuji Hao, etc.

# Intro

relative similarity learning (RSL)的目标是为了在一定的限制条件下，从训练数据中学习到similarity function。现有的RSL有这么几个问题

1. 需要大量的re-training，因为使用batch-based method
2. 前面提出的online learning主要是为了single-task。所以对于multi-task的设定下，要么对每一个task独立学习一个local similarity，要么对于所有的task学习一个global similarity。

但很少有工作研究multi-task relative similarity learning，最类似的是mtLMNN。mtLMNN能够对于multiple task上学习一个global metric和local metric。mtLMNN有几个问题，一个是batch learning，因此比较耗费时间；另外是mtLMNN强制学习了一个positive semi-definite （PSD） distance matrix；最后mtLMNN是假设有explicit labeling，而在实际中很难实现。

# Online Multi-Task Relative Similarity Learning (OMTRSL)

这里同时学习K个similarity function，对应K个相互关联的tasks。定义为：$$S^k (x,x') , k\in [K]$$，其中 $$x,x' \in \mathbb{R}^d$$，是第k个task的两个instance。假设有一个bi-linear form的similarity function

$$S^k(x, x') = x^T M^k x'$$

对于online multi-task relative similarity learning，我们收到了triplet data，分别有三个部分：三个sample组成的training data，相关的label，已经task的index。label是1 表示$$x_t$$和$$x_t^1$$比$$x_t^2$$更加相近；否则如果是-1 表示与$$x_t^2$$更加相近。online multi-task relative similarity learning的目的是为了同时学习K个similarity matrices $$M^k$$，从而给相近的实例更高的score。

## Proposed Method

上面描述的就类似self-paced的步骤，不过对象换成了triplet。

loss function定义在论文中。定义一个task similarity matrix $$A \in \mathbb{R}^{K \times K}$$，$$A$$的逆就是$$M$$的learning rate。

$$
w_{t+1} = w + y_t \tau_t A_{I}^{-1} \phi_t \\
M_{t+1}^k = M_t^k + y_t \tau_t A_{k,t_k}^{-1} X_t
$$

## Active Learning Extension

使用stochastic active sampling scheme来觉得是否要采用当前这个triplet用来update。简单地说，就是使用伯努利分布，如果$$Z_t=1$$来确定是否要在第t步，使用true label $$y_t$$；否则$$Z_t=0$$就不采用。

## Theoretical Analysis

证明了一下算法的error bound。

# Appendix

