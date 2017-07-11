# Pentuum: A New Platform for Distributed Machine Learning on Big Data

Eric Xing, Qirong Ho, Wei Dai, Jun Kyu Kin, Jinliang Wei, Seunghak Lee, Xun Zheng, Pengtao Xie, Abhimanu Kumar, and Yaoliang Yu

CMU, Institue for Infocomm Research

和已有的一些模型进行比较：Spark的容错和恢复非常出色，GraphLab侧重于本地和全局的一致性。

Pentuum引入了三个新的目标，来加速convergence：
+ 在保证有限过期(bounded stleness)前提下，同步parameter states
+ 动态调度，以便能够考虑到模型参数的结构改变(比如dropout)
+ ML算法中的参数呈现了非一致的convergence cost(比如converge的步数不一样)，Pentuum优先计算non-converged的模型参数，以便更快的converge

data-parallelism，就是把数据分散到几个不同的机器上，是large-scale常用的手段之一。model-parallelism,把模型进行划分。
+ data-parallelism最重要的是，每个机器可以在本地进行aggregation，然后再通过网络传递update之后的model weight。能够将数据进行划分的一个重要假设就是数据是iid的。parameter server就是一种data-parallel的方法。
+ model-parallelism则需要在每次更新weight的时候，只能限制在一个weight subset上进行更新。这种选择model subset进行更新的方法叫做scheduling调度。因为模型的参数之间相互是有联系的，所以iid的假设在这里不成立。model-parallel算法只有在independent parameter情况下适用。

ML的模型是stateful，但是MapReduce框架式non-stateful。对于model-parallel，需要很好的控制parameter-scheduling来避免non-convergence，Hadoop、Spark或者GraphLab就做不到。

Petuum提供了接口来实现data-parallelism和model-parallellism。主要设计的两个部分parameter server和scheduler。

# Petuum Parallel Algorithm

举了几个例子。

## DML

一个是DML，distance metric learning。在NIPS 2002中有这篇paper，和Andrew Ng etc合作。

DML主要是优化一个Mahalanobis distance matrix，通过引入slack variables能够解决。

## model-parallel Lasso

最简单的parallel CD(coordinate gradient) Lasso使用model-parallel进行模型subset update。

API:用schedule()选择依赖性比较低的参数，来加速converge。

## other algorithms

+ Topic Model(LDA):最重要的参数是"word-topic" table，需要所有的worker一起更新。我们同时使用data-and-model-parallel，每一个worker的数据都是没有任何交集的subset。
+ Matrix Factorization(MF):可以使用并行的方法进行large matrix high-rank decomposition。
+ Deep Learning(DL):实现了两种，使用cross-entropy loss的DNN，和针对image classification的CNN。

# Appendix