# TensorFlow: A system for large-scale machine learning

Google Brain的一篇文章。

# Background

这篇paper拿了两个例子 image classification和language modeling

2012年的时候，训练一个image classification需要用16k CPU，三天；而2个GPU则6天。然后，GPU的厂家开始支持机器学习的训练：NVIDIA的cuDNN库。

除了一般化目的的设备，还有一些特殊目的的硬件。比如谷歌的TPU（Tensor Processing Unit），转为机器学习设定，并且数量级地提高性能（每消耗1瓦特的性能）。其他加速器，比如Myriad 2 processor，FPGA（Field programmable gate array）。

**Single-machine frameworks** 许多算法是部署于一台机器上的，通常是GPU支持。Caffe是特地为了跑convolutional neural network的高性能框架，跑多核CPU或GPU。Theano将模型描述成一个图，然后产生高效的编译代码。Torch则是特地为科学计算提供了细粒度控制。

**Batch dataflow systems** 从MapReduce开始，batch dataflow就应用到了机器学习上。DryadLINQ增加了高层次的查询语句，支持更复杂算法（但是底层还是通过MapReduce实现），Spark将DraydLINQ拓展到cache前面计算的数据在内存中，对应iterative算法比较有用。Dandelion将DryadLINQ拓展到支持GPU和FPGA。

**Parameter servers** 我们前面一个系统，DistBelief和parameter server的框架类似，不过parameter server用在了topic modeling上，我们用在训练deep neural netowork上。GeePS证明了parameter server和GPU兼容，并且可以加速。

MXNet是最近一个使用parameter server来压缩训练、支持GPU加速，并支持多种语言。但缺点是“具有优先权的代码”，使得它很难拓展到其他模型。

DistBelief拓展性不好，因为增加一个新的算法或者洗的模型结构，会要求用户修改parameter server的实现。TensorFlow对用户支持灵活的实现。

# TensorFlow execution model

用一张数据流图来表示所有的计算和机器学习算法的计算和状态。dataflow TensorFlow和batch dataflow系统的区别：

+ TensorFlow支持subgraph的多并发执行
+ 每一个节点可以有易变的状态，可以在graph不同的执行之间共享

在大数据训练上，可改变的状态变得重要，因为可以做原地更新。在TensorFlow图中，每一个节点都是一个计算原子，每一条边代表了输入和输出。讲每一个节点的计算叫operations，每条边上传输的数值叫tensors。

**Tensors**：所有的数据都被当做tensor，是非常密集的n维数组，每一个元素都是primitive types。TensorFlow里的所有Tensor都是密集的。为了表示稀疏的，TensorFlow提供了两种方式：将数据表示成variable-length 字符串，或者用几个dense tuple of tensors。

**Operations**：接收m个tensors，输出n个tensors。