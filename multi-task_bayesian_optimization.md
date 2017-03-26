# Multi-Task Bayesian Optimization

Kevin Swersky

Jasper Snoek

# 1 Introduction

很多人开始用bayesian optimization的方法选取参数。hyperparamer的选取可能决定了这个模型是poor还是state-of-art结果。随着ml模型变复杂，hyparameter也变得更加重要。

Bayesian optimization的一个问题是冷启动。当一个模型训练在一个新的数据的时候，一切都要重新开始。如果一个模型应用到不同的数据集，或者只是几个特别大的数据集，那么在进行新的hyperparamter search的时候就要花费大量的时间。一种解决的方案是将一个domain运用到另一个。比如使用grid search，或者使用一些常用的数值。这个是区分ML专家和菜鸟的区别。（这样说真的好吗）

这篇文章探究的就是我们是否能将在一个Bayesian optimization框架中，把同一类的knowledge transfer合并到一起。也就是说，对于大数据集，可以在subset上探究大量的hyperparameter，然后再使用这个knowledge快速找到对应整个dataset适用的hyperparameter。代价仅仅是几个评估函数。

提出multitask Bayesian optimization解决这个问题。基本想法是，将学习的很好的multk－task Gaussian process models延用到Bayesian optimization框架中。如果有新的task作为domain进来，就迭代式的学习domain之间的关联度，并且用这个信息来训练搜索算法。

# 2 Background

## 2.1 Gaussian Process

假设任意一个有N个点的有限点集$$X=\{ x_n \in X' \}_{n=1}^N$$都符合$$R^N$$上的高斯分布。因此有了(1)和(2)的公式。

$$K(X,x)$$是N维的列向量，表示$$x$$和$$X$$之间的cross-covariances.$$N \times N$$ matrix$$K(X,X)$$是$$X$$的[Gram matrix](https://en.wikipedia.org/wiki/Gramian_matrix)。

## 2.2 Multi-Task Guassian Process

multi－task的情况下，最重要的是不同task之间的covariance函数$$K((x,t), (x',t'))$$。一种做法是intrinsic model of coregionalization，使用一种潜在模型来产生输出。

$$K_{multi}( (x,t), (x',t') ) = K_t(t,t') \times K_x(x,x') $$

$$\times$$代表的是Kronecker product，$$K_x$$测量的是inputs之间的关系，$$K_t$$测量的是tasks之间的关系。这就是一个标准的Guassian Process。

## 2.3 Bayesian Optimization for a Single Task

Bayesian optimization的策略就是可以使用一个相对简单的probabilistic model来代理一个金钱、计算和物理上都非常昂贵的函数优化。Bayes用来根据观测得到真实函数的后验估计。代理surrogate就进行下一步最有可能的查询。常用的方法是为了最小化某一个loss，根据观测数据，使用一个GP来找到一个分布。也就是说，给定观测值$$\{x_n, y_n \} _{n=1}^N$$，假设$$f(x)$$服从于Gaussian process prior，$$y_n \sim N(f(x_n),\nu)$$,$$\nu$$是噪音。

# 3 Multi-Task Bayesian Optimization

## 3.1 Transferring Bayesian Optimization to a New Task

在multi－task GPs的设定下，在相关的task上进行优化是非常直接的。只要把要观测到的模型当作normal。当有足够的数据在新的task上观测到合适的估计$$K_t$$，那么其他的task就会像额外的观测值，而不需要额外的函数估计。

## 3.2 Optimizing an Average Function over Multiple Tasks

这里考虑在multiple task上优化平均函数。

$$\bar{\mu} (x) = \frac{1}{k} \sum_{t=1}^k \mu(x,t;\{x_n,y_n\},\theta)$$

$$\bar{\sigma} (x)^2 = \frac{1}{k^2} \sum_{t=1}^k \sum_{t'=1}^k \Sigma(x,x,t,t';\{x_n,y_n\},\theta)$$

## 3.3 A Principled Multi-Task Acquisition Function

从已有的task上学到的knowledge转移到新的task，与这个方法不同的是，可以有一个optimization routine，动态的查询可能cheaper的task。也就是，如果两个task高度相关，那么评估cheaper的那一个能在揭示信息的同时减少不确定性。

# 4 Empirical Analyses

## 4.1 Addressing the Cold Start Problem

**Branin－Hoo** 是常用的benchamrk函数。

**logistic regression** 在MNIST数据集上，使用10k validation养老，优化四个hyperparmeter。

**CNN on pixels** 使用Street View House Numbers(SVHN)数据集。

**convolutional networks on k-means** 在STML－10数据集上。

## 4.2 Fast Cross-Validation

k－fold cross validation是用来评估ml模型常用的算法，但需要训练一个模型k次，对于大数据集和复杂的模型比较昂贵。也有理由相信，如果数据在不同的partition中是随机分布的，那么每一个fold的error会高度相关。对于一个给定的hyperparameter，我们可以假设每一个fold的平均error会逐渐变小。有一个非常好的GP模型时，我们可以仅仅在一个fold上就获取非常高质量的评估。

## 4.3 Using Small Datasets to Quickly Optimize for Large Datasets

使用multi－task entropy search strategy（MTBO）进行优化，找最佳hyperparameter set。

# 5 Conclusion

随着数据增加，模型变复杂，有必要使用新的策略来尽快tune parameters。Bayesian optimization就是一种解决方案。

# Appendix