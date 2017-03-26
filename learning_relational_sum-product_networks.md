# Learning Relational Sum-Product Networks

Aniruddh Nath, Pred Domingos

UW-Seattle

Sum-product networks(SPNs)是最近提出的，一种exact inference方法。

# Background

## Sum-Product networks

sum-product networks是有向无环图，叶子节点有统一的分布。内部节点是sums和products。

## Learning SPNs

最开始针对sum-product networks的learning alg是使用一个固定的network structure，所以只要优化weights。这个网络框架是依赖于domain的。

最近提出了一些方法，使得能够同时学习structure和weight。(也就是同时能做到learning和inference)有自下到上，也有自上到下的方法。

## Statistical Relational Learning

SPNs假设实例都是iid，但是这个假设在实际中通常不实用，因为对象之间通常有相互依赖的关系。Statistical Relational Learning算法能够抓住对象之间的关系，并且进行预测。

Markov Logic Networks(MLNs)是广为接受的relational learning代表。MLN的局限是产生的模型high-treewidth，而inference是无法追踪的。实际中，SRL更多的是用approximate inference算法，比如MCMC或者loopy belief propagation，更加长的运行时间和难以确定的预测。

Tractable Markov Logic(TML)能保证polynomial-time inference，哪怕是high-treewidth的情况下。TML的局限是knowledge base充分的展示了domain中可能出现的对象和所属的类、结构。

# Relational Sum-Product Networks

## Exchangeable Distribution Templates

在定义RSPNs之前，先定义Exchangeable Distribution Template(EDT)。

定义finite exchangeable set:

考虑一个有限的变量集合$$\{ X_1, ... ,X_n \}$$，联合概率分布是$$P$$。$$S(n)$$是$$\{ 1, ... ,n \}$$的任意排列。$$ \{ X_1, ... ,X_n\}$$是关于$$P$$的finite exchangeable set，当且仅当$$P(X_1, ..., X_n) = P( X_{\pi(1)} , ... , X_{\pi(n)} )$$，对于任意$$\pi \in S(n)$$。

定义exchangeable distribution template(EDT):

EDT是函数，能够把$$\{ X_1, ... , X_n \}$$作为输入，返回一个联合概率分布$$P$$，而输入的$$\{ X_1, ... , X_n \}$$是exchangeable。

## Relational Sum-Product Networks

RSPNs将对象的属性和关系进行联合建模。RSPNs继承了TML关于part和class的概念。但是和TML不一样的是，RSPN的class是unique和exchangeable。

# Appendix

参考的[这篇](http://freemind.pluskid.org/machine-learning/probabilistic-graphical-model/)

Inference:在给定一个多变量的联合概率分布(joint distribution)情况下，计算某些变量的边际概率分布(marginal distribution)。整个模型都是已知的。

Learning:分几种情况
+ 已知模型的结构，但是不知道具体参数。解决方案是Maximum Likelihood Learning。
+ 模型的结构也不知道，希望从数据中学习到。典型的解决方案是定义一个structure score，然后选出score最高的。最直接的是用likelihood score，定义为该graph structure下单maximum likelihood estimation模型下得到的likelihood数值。

exact inference分为两种，求边缘概率和MAP，分别对应sum-product和max-sum算法。