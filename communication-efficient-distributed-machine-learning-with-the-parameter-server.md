# Communication Efficient Distributed Machine Learning with the Parameter Server

<p align="center">Mu Li, David G. Andersen, Alexander Smola, Kai Yu</p>

李沐这篇文章介绍的是第三代parameter server。提出了一种解决non-convex，non-smooth问题的新算法。

# 1 介绍

工业中的数据经常是由1TB到1PB，参数也会到达$$10^9$$和$$10^12$$这个级别，一台机器很难及时地完成学习任务。

分布式的优化方法以及是解决大规模机器学习问题的重要工具：将workload分到了不同的worker machine，每个worker machine都会访问全局共享的模型，然后各自同时进行local computation来优化模型。会遇到这么两个问题：

+ worker machine必须经常读写全局共享的参数，很可能占用大量的网络带宽。而网络带宽往往是datacenter里面最稀缺的资源，所以网络带宽往往变成了分布式优化算法的瓶颈。

+ 许多优化算法是顺序的，会需要worker machine之间经常性的同步。同步通信时，所以通信的机器要等最慢的机器。这个在workload分配不均匀、网络拥堵、或者其他运行任务影响的情况下，机器很可能会变慢。又是一个瓶颈。

先比较了一下当前几个系统能处理的数量级：

 系统 | # of shared parameters | # of cores
----|------|----
Parameter Server | $$10^{10}-10^{11}$$ | $$10^4-10^5$$
Petuum | $$10^{8}$$ | $$10^3$$
YahooLDA | $$10^{7}-10^{8}$$ | $$10^3-10^4$$
Graphlab | $$10^{5}-10^{6}$$ | $$10^3$$

paper里面有一张很详细的图，这里先简单的列出。从图表中展示的，parameter server在数量级上比其他都要高。用了异步通信提高通信性能，作为代价，数据的不一致性和算法converge rate之间有一个trade-off。这个系统用了两种relaxation来解决一致性：1.与用一个单独的model不一样，这里支持了一系列model 2.系统允许用户指定的过滤器来进行细粒度的一致性管理。

拿一个regularized optimization problem作为例子：

> $$\underset{w}{min} F(w) \text{where } F(w) = f(w) + h(w), \text{and } w \in R^p$$

> Assume $$ f :R^p \to R $$ is continuously differentialble, but not necessarily convex, and regularizer $$h: R^p \to R$$ is convex.

这篇paper提出的算法是基于proximal gradient method来解决这个问题，这个改进的算法和proximal gradient method的区别有以下：

+ 每次update只用了一个subset（block） of coordinates
+ 因为异步通信，每个worker维护的model和其他机器维护的model只是部分一致
+ proximal操作用了coordinate-specific学习速率来应用在稀疏数据上
+ 只有会改变模型权重的coordinate会被交流，从而减小了网络消耗

# 2 背景

## 相关工作

大公司都有在做parameter server框架，Amazon，Baidu，Facebook，Google，Microsoft和Yahoo!开源的项目则有YahooLDA，Petuum（Eric Xing）。

第一代parameter server：缺少灵活性和性能。仅使用键值对存储作为同步机制。YahooLDA增加了一个专门的服务器来改进这个机制，是用户能自定义地更新操作。<br/>
第二代parameter server：第二代则特定针对了应用。而Petuum改进YahooLDA，来试图general，但在线程模型上有了更多的限制。<br/>
第三代parameter server：性能、灵活性、容错性都能够做到。
[参考](http://www.learn4master.com/machine-learning/parameter-server/parameter-server-resources)

除了parameter server，其它还有一个general的分布式系统也能支持ml应用。比如Mahout（基于Hadoop）MLI（基于Spark）两个都采纳了迭代的MapReduce框架。而GraphLab支持全局的参数同比。这些在几百个节点的时候表现不错，但是当大范围的同步时，性能就会到达瓶颈。

## parameter server架构

包括一个server group和几个worker group，每一个group都有好几台机器。server group的每台机器都保存部分的全局参数，而server之间相互交流，来复制、传递参数，以此提高可靠性和范围。

每一个worker只存储部分的training data，然后计算local gradient。worker只和server交流，来获取和更新参数。每一个worker group，都会有一个调度机器scheduler machine，它给每一个worker分配活并且监察进程。当worker加入或退出group，scheduler会重新安排未完成的任务。每一个worker group跑一个任务，以此来满足multi-tenancy。比如一个ad-serving system和一个inference system推理系统可以同时在两个group里跑。

共享的参数以键值对`(key, value)`的形式保存。我们可以把这个当作稀疏的向量或者矩阵，通过内置的多线程线性代数函数和training data进行交互。数据交换通过两个方法：push和pull。一个worker可以从一个server在一定范围内push或者pull任意键值对。

## 分布式的subgradient descent

training data已经被划分并且分布到了不同的worker上。模型$$w$$是递推地学习的，在每一次的迭代中，每个worker都用本地的training data计算local gradient，然后server会把这些gradient合并起来，来更新全局共享的参数$$w$$。然后worker会从server那里取回更新后的权重。

高维的数据很有可能在worker里面不fit，而这类数据一般会比较sparse，然后一个worker只需要model的一个子集，而且这个子集通常比较小。

paper中有一段伪代码解释的很好，可以参考。

# 3 数据一致性的两种relaxation

## 3.1 异步任务依赖

将parameter server的workload分解成由caller给callee发布的任务。任务有很大的灵活性，可以是worker对server的push或者pull，或者scheduler给node发出的用户自定义函数（比如分布式subgradient descent的每个迭代）。

task的异步执行：caller可以在发出任务之后立即进行计算。caller在收到callee的回复后一个任务标记为完成。一个回复可以是用户自定义的函数回复、由pull请求的键值对、或者一个空的确认。callee会在subtask完成之后，task的call返回，标记为完成。

callee会并行计算tasks。caller则在task之间加入_execute-after-finished_的依赖。paper中的图给了一个很好的例子。

## 3.2 在依赖图上实现灵活的一致性模型

上面提到的依赖关系图可以用于放宽一致性要求。独立的任务通过并行（CPU，disk，network）来提高效率，但也会带来节点间的数据不一致性。还是图中的例子。图中的例子会使得两次迭代的结果一样，但是有的算法会对数据不一致性不那么敏感，比如每次迭代只使用one block of data，那么只有一部分收到了影响。

算法的效率和系统的性能依赖于许多因素，比如特征关系、硬件容量、数据中心负载等等。parameter server提供很高的灵活性，通过给不同的一致性模型建立不同的任务依赖图，这个依赖图是有tasks之间的依赖性定义的DAG。这么三个例子：

+ sequential consistency：其实就是同步的任务，只有上一个完成才能进行下一个
+ eventual consistency：所有任务相互独立，那么就可以同时开始
+ bounded delay：介于sequential和eventual中间，设置了一个最大延迟时间$$\tau$$，只有之前的任务在$$\tau$$时间完成了，才能开始新的任务。section 4会介绍这个方法的应用。这个$$\tau$$是可以动态改变的。

## 3.3 用户自定义过滤器实现的一致性模型

对优化问题，比如梯度下降，并不是每次计算的梯度对最优化都是有价值的，可以通过自定义的filter过滤掉一些不必要的传输，压缩带宽cost。

用户自定义的过滤器可以更加细粒度地控制一致性。一个filter可以改变和有选择性的同步键值对。几个可以合起来提升data compression的filter：

+ significantly modified filter：只有改变超过了阈值，才push
+ random skip filter：随机选择
+ KKT filter：采用了optimality condition，只在非最优情况下发送
+ key chaching filter
+ compressing filter：因为大部分传送的数据都是0、small int、和float，可以进行无损压缩

# 4 Delayed Block Proximal Gradient Method

这个bounded delay的PGD（proximal gradient descent）就是提出的算法。

**Proximal Gradient Method** 对一个近似convex的函数$$h(x): R^p \to R$$，定义generalized proximal operator：

$$Prox_\gamma^U (x) := \underset{y\in R^P}{argmin} \, h(y) + \frac{1}{2\gamma} \| x-y\|_U^2$$, where $$\|x\|^2_U = x^T U x$$

Mahalanobis norm马氏范数$$\|x\|_U$$要求$$U$$正定，许多proximal算法直接$$U=I$$。PGD用了以下两步进行更新$$w$$：向前步计算f的steepest gradient descent，向后步则用h进行投影。如下：

$$w^{(t+1)} = Prox_\gamma^U[w^{(t)} - \gamma_t \bigtriangledown f(w^{(t)})]$$， for $$t = 1, 2, 3 ...$$

**Algorithm** 这里通过每次更新block的做法，来放宽一致性要求。算法的描述在_Algorithm 2_中描述，和前面比较直接的做法有如下不一样的地方：

1. 每次参数更新都只是更新一个block（子集）
2. wroker both计算gradient和coordinate-specific learning rate，比如二次求导的对角线
3. 迭代是异步的，用了bounded-delay模型
4. 应用了用户自定义filter来压缩传输（对模型几乎不会产生影响的部分）

# Appendix

网上还是有一些分析很不错的文章，[链接](http://www.learn4master.com/machine-learning/parameter-server/parameter-server-resources)
