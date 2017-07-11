# Parameter Server for Distributed Machine Learning

<p align="center">Mu Li, Li Zhou, Zichao Yang, Aaron Li, Fei Xia, David G. Andersen and Alexander Smola</p>

parameter server的框架，将数据和工作分布到client node，而在server node上进行全局共享参数的维护，通常参数是稀疏的向量或矩阵。框架在client和server间采用了异步通信。这个框架支持灵活的一致性模型、弹性的规模和容错能力。

# 1 介绍

分布式的优化变得越来越火。实现分布式的算法需要考虑密集的工作负载和大量的数据交流。现实情况下（实际云计算平台中）机器会宕、工作会被抢占、数据会丢失、网络延迟和临时负载会导致更加复杂的性能评估。这要求更加robust的解决方案。

已经有了很多解决方案，比如Mahout（基于Hadoop）MLI（基于Spark）两个都采纳了迭代的MapReduce框架，也都使用了同步的迭代模式，使得两个方法在迭代的机器学习算法中都会产生不一直的性能分布。为了克服这个缺点，distributed GraphLab通过图抽象的方法用异步通信的调度。但是这个方法缺少MapReduce框架弹性的拓展性，并且依赖于粗粒度coarse-grained的快照来恢复。

发现许多推理问题都有相当有限的结构，在参数化方面，利用参数的设计就可以有比较大的改动。比如generalized linear model用一个大规模的参数向量，topic model用一个稀疏的向量数组。整体来说，大多数大规模的graphical model大部分是由少了的木板plate(?)组成，因此允许使用重复性的结构，这些结果可以在observations和machines之间分享。

这篇paper将parameter server的重心放在了分布式优化上。所有的计算节点分做了client和server。每个client有部分的数据和工作量，server一起维护全局共享参数。设计的目的是：

1. Ease of use：全剧共享参数是以向量和矩阵的形式标记的，对ml应用比以键值对或者表格形式存储的数据结构好用。高性能的多线程操作，比如向量-矩阵乘法，都能用来进行很好的支持。
2. Efficiency：节点间的通信是异步的，同步（迭代的操作）也不会阻塞运算。这就使得算法设计这可以在convergence rate和system efficiency之间做一个trade-off。
3. Elastic Scalability：新增加一个节点不用重启整个系统，用分布式的哈希表来使得节点可以动态加入。
4. Fault Tolerance and Durability：宕机是不可避免的，调度器的强制回收会加剧这个现象。用优化的数据复制架构，将数据copy保存在不同的服务器节点来提供快速恢复。（小于1s）而且client是相互独立的，新的client可以在某一个宕机之后自动启动，和MapReduce重新安排mapper一样。

# 2 架构

## 2.1 概观

server节点保存全局共享参数的划分，彼此之间复制和（或）转移参数来提高可靠性和规模。client节点进行大量计算；server主要进行bookkeeping和参数的全局聚合工作。每一个client都保存一部分本地的迅雷数据、计算gradient。client只和server交流，更新并获取新的共享参数。

parameter server同时支持多个独立的不同模型（channel）的parameter vector。这个的有用之处在于可以存储一些经常被使用的操作模型的参数，每当算法要调用这个操作（模型）的时候，只要转换channel，用不同的client节点就好。

## 2.2 应用例子

下面给几个use case

**Risk minimization by distributed subgradient iterations**

**Risk minimization by parameter synchronization**

**Distributed Gibbs Sampler**

**Deep Learning**

**Sketches**

# 3 接口

## 3.1 键值对向量

和现有的方法比，我们不一样的地方在于假设index set of keys有序并且密集的。这意味着可以发送大块的数据而不是以单个的键值对形式，传输的数据更多。

parameter server将参数以稀疏的vector的形式给client和server，应用则会按照自己的方便，或者用向量、矩阵的形式，或者用键值对的形式处理数据。client和server可以在整个vector上面采用线性代数的操作，比如假发、求2范数、或者更一般化的$$\alpha A x + \beta y$$操作。

接口的设计会使得代码更加高效，充分利用向量和矩阵的结构，线性代数操作就可以优化，已经有类似BLAS/Lapack的库做了这方面内容。也更容易多线程的内部实现这些操作，利用SIMD/vector support。

## 3.2 Push和Pull

节点之间的数据交流通过两种方式：push和pull。前者将本地修改的共享参数数据发给别人，后者则是接受远程修改的参数。应用可以定义什么时候用新的本地数据（例子中的client），或者刚刚更新的数据（例子中的server）。

通过只发送needed数据，parameter server减少网络使用。比如server维护的是共享参数的一部分，当一个client push的时候，找到本地更新的数据，然后只发送这些需要更新的数据给server，并维护这些数据的key。client则只需要发送pull request中提到的或者前期和server沟通好的list of specific keys。

这两个操作都是non-blocking。caller（通常是计算线程）将请求插到队列中，然后重新开始计算。这种异步通信对数据的一致性要求比较高，section 4.1会进一步解释。

## 3.3 server上的用户自定义函数

server上的用户自定义函数是因为进程需要对共享参数进行更新。比如proximal gradient methods，每一次迭代的时候，都先得聚合loss function的gradients，然后通过proximal operator计算一个新的权重。如果用$$l_1$$ regularizer，这就使soft-shrinkage operator。在server端的函数来计算proximal operator，而不是client结点，可以减少节点间传输的数据。在sketch中也是类似的道理，client几乎不进行计算，最大的份额在server上。

# 4 规模和可靠性

## 4.1 一致性模型

异步通信提高了效率，但是也带来了数据不一致性。在有效性和算法convergence rate直接最好的trade-off都决定于许多因素，比如算法对数据不一致性的敏感程度，特征相关性，和硬件容量的差别。parameter server提供了这么几种数据一致性模型：

**Best Effort**： parameter server在这种情况下，会不顾资源的可靠性，一直进行下去

**Maximal Delayed Time**：设置了一个最大延迟时间$$\tau$$，只有之前的任务在$$\tau$$时间完成了，才能开始新的任务。section 4会介绍这个方法的应用。这个$$\tau$$是可以动态改变的。

**User-defined Filters**：对优化问题，比如梯度下降，并不是每次计算的梯度对最优化都是有价值的，可以通过自定义的filter过滤掉一些不必要的传输，压缩带宽cost。用户自定义的过滤器可以更加细粒度地控制一致性。一个filter可以改变和有选择性的同步键值对。

## 4.2 弹性可拓展性和容错能力

从键值对的角度看共享参数。基本想法来自于分布式的哈希表，键值对和server节点都插入到了hash ring中。每一个节点都管理按照逆时针方向的一段的key，叫做逆时针邻居anticlockwise neighbour。将key segment到node的映射保存在Paxos（可容错的一致性协议，zoopeeper实现）

每一个key segment知乎就被复制到了k个逆时针方向的邻居服务器节点里，以此提供容错性。一个新的节点插入是，先通过哈希函数随机的查到ring中，然后讲它顺时针方向的key segment拿过来。如果删除一个节点，它的segment会用来服务最近的逆时针邻居，而这个逆时针邻居已经有它的一份数据。要恢复节点，就把这个节点插回到ring中，然后从逆时针的邻居请求对应的key segment。

# 5 理论分析

## 5.1 nonconvex和nonsmooth优化

一下面的nonconvex优化问题为例子

$$\underset{w}{min} \text{ }F(w) := f(w) + h(w), \text{and } w \in X$$

$$f$$是连续可导，但不一定凸，$$h$$是凸，但不一定连续。

用proximal gradient descent解决，给定一个凸函数$$h(w)$$，定义proximal operator如下：

$$Prox_\gamma (x) := \underset{y\in X}{argmin} \text{ } h(y) + \frac{1}{2\gamma} \| x-y\|_2^2$$

为了优化目标复合函数，PGD用了以下两步进行更新$$w$$：向前步计算$$f$$的steepest gradient descent，向后步则用$$h$$进行投影

$$w^{(t+1)} = Prox_\gamma[w^{(t)} - \gamma_t \bigtriangledown f(w^{(t)})]$$， for $$t = 1, 2, 3 ...$$

## 5.2 异步通信和convergence guarantee

这里以每个server节点作为一个基本单位来简化讨论。$$h$$可以是$$l_1$$ penalty。

首先，假设数据分到了m个client，所以$$f(w) = \sum_{i=1}^m f_i(w)$$。然后在每步迭代，每一个client i同时计算本地导数$$\bigtriangledown f_i$$然后将它传给server。server收集到了这些更新的数据后，在计算proximal operator之后将更新的$$w$$发回给client。

下面解释一下论文中的推导

对于proximal gradient method，可以得出

$$ 0 \in \partial h(y) - \frac{1}{\gamma} \|x-y\| $$

然后根据定义，$$w^t$$和$$w^{t+1}$$是符合这个式子的，那么将$$x=w^t$$和$$y=x^{t+1}$$代入，得到

$$\frac{1}{\gamma}(w^t-w^{t+1}) - s^t \in \partial h(w^{t+1})$$

然后根据convexity：$$f$$如果是convex函数，那么符合$$f(x) \ge f(y) + \bigtriangledown f(y)^T(x-y)$$

所以代入$$h$$中，得到：
$$h(w^{t+1}) - h(w^t) \le (\frac{1}{\gamma}(w^{t+1}-w^t) - s^t)^T(w^{t+1}-w^t)$$

同理，我们有

$$f(w^{t+1}) - f(w^t) \le \bigtriangledown f(w^{t+1})^T(w^{t+1}-w^t) =  \bigtriangledown f(w^{t})^T(w^{t+1}-w^t) + (\bigtriangledown f(w^{t+1})-\bigtriangledown f(w^{t}))^T(w^{t+1}-w^t) $$

And as assumed, we get $$f$$ is Lipschitz-continuous, we can get $$\bigtriangledown f(w^{t+1})-\bigtriangledown f(w^{t}) \le L \|w^{t+1} - w^t\|$$ 

Put this back to what we get:

$$f(w^{t+1}) - f(w^t) \le  \bigtriangledown f(w^{t})(w^{t+1}-w^t) + L \|w^{t+1}-w^t\|^2 = \bigtriangledown f(w^{t})^T \delta(t) + L \|\delta(t)\|^2  $$

Since $$F = h + f$$, we get(using Cauchy inequality)

$$F(w^{t+1})-F(w^t) \le (L-\frac{1}{\gamma}) \|\delta(t)\|^2 + \delta(t)^T (\bigtriangledown f(w^{t}) - s(t)) \le  (L-\frac{1}{\gamma}) \|\delta(t)\|^2 + \|\delta(t)\| \cdot \|\bigtriangledown f(w^{t}) - s(t)\|$$

（竟然写着写着就不小心当作业了，将就看一下……）

$$s(t)$$是gradient的近似值，因为这里用了maximal delayed time，然后按照这个近似值的定义，得到了$$\|\bigtriangledown f(w^{t}) - s(t)\|$$的上界，后面的有点卡，**mark一下**，下次回来推。

# Appendix

先补充一下，这篇文章每次引用的时候，作者都标为是_Big Learning NIPS Workshop, 2013_，并不是conference文章。

然后也可以在网上找到各种关于parameter server的文章，这篇出现的频率相当高，比如[这里](https://www.zhihu.com/question/26998075)，另外还有[仙道菜在CSDN上的一篇详解](http://blog.csdn.net/cyh_24/article/details/50545780)。

至于MXNet也是最近很火的内容，下面我应该也会继续读相关的paper。