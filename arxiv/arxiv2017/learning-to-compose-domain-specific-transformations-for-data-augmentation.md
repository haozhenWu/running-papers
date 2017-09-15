# Learning to Compose Domain-Specific Transformations for Data Augmentation

Alexander J. Ratner, etc.

# Modeling Setup and Motivation

以往augmentation的方法，会要求将每个instance augment之后，还能够保持同样的label比率。这个假设难以保证。

这里提出了一个弱一点但是更加可靠的假设 weaker invariance assumption，每一个instance augment之后，只要保证符合同样的label分布即可。

使用GAN来进行data augmentation。discriminator用来判断生成的数据是否在期待的label分布中，而generator用来生成instance sequence，同时让discriminator无法判别是否是真实数据还是生成数据。

生成器有两种类别：

**Independent Model:** 考虑mean field model，每一个transform function都是独立的选择。（也就是生成的序列不存在前后关系）这对于离散随机的transform function，学习特别方便。

**State-Based Model:** transform function之间也会有某些效应，比如同时的模糊和放缩处理会给数据带来损失。mean field model无法将这些考虑在内。因此，使用LSTM来对这些transform function加上序列关系。

# Learning a Transformation Sequence Model

将问题放到RL的框架中，能更好的处理non-differentiable或者随机的节点。

按照文中的标记，在每一个时间点$$t$$结束之后，$$s_t$$记录了所有从第一个时刻到现在的transform function和生成的datapoint。然后将$$s_t, \tau_t$$放入LSTM中进行训练。可以看出这是一个incremental过程。

既然是incremental过程，那么可以得到一个非常reasonable的reward，将当前时刻的loss减去上一个时刻的loss。

所以如公式6，就有一个非常优雅的accumulative loss表达，即为第一个时刻（也就是每一个epoch，G生成的datapoint）的loss，然后一直累加后面t时刻的rewards。

通常第一个时刻的loss会忽略。所以policy gradient表达就如公式7。这里简单的推一波，主要是根据REINFORCE，[Karpathy's blog](http://karpathy.github.io/2016/05/31/rl/)

$$\bigtriangledown_\theta \mathbb{E}_{\tau \sim G_\theta} \mathbb{E}_{x \sim \mathcal{U}} [ \sum_t R(s_t)] = \mathbb{E}_{\tau \sim G_\theta} \mathbb{E}_{x \sim \mathcal{U}} [\sum_t R(s_t) \bigtriangledown_\theta log \, \pi_\theta(\tau_t)] $$

# Experiment

在MNIST，CIFAR-10，ACE和DDSM上测试。表1展示了mean field和LSTM两中方法在不同的数据集上，不同的eval方法都有提升。

# Appendix

这应该是最近读到的第二篇用RL和GAN的paper。RL这个框架的适应力太强，也的确很方便。