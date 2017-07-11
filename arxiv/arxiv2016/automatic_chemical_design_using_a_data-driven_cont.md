# automatic chemical design using a data-driven continuous representation of molecules


主要做的一件事情是，用deep neural network训练了大量的chemical structure，来构造了两个成对的函数：encoder和decoder。encoder将离散的molecule表示转换成连续实数组成的向量，decoder将这些连续的向量转换回离散的表示。这些向量组成了一个latent space，也就是encode的结果、decode的输入。（但是latenct space我谷歌了，是LDA的一个概念，[链接](https://www.quora.com/What-is-the-meaning-of-latent-space))

连续的表示有这么几个好处：

1. 能够在latent space上面生成新的chemical structure，有点像matrix completion，给定了几个比较有意思的已知结构，然后每个向量都已经有了确定的几个实数，把missing部分补充完整就可以生成新的chemical structure。
2. 连续的表示可以用优化算法进行求解。gradient-based

# Introduction

molecular graph的表示是离散的，可以是三维的原子的分布或者是一个无环图，原子是节点，bond是边。从化学信息学的角度，这些表示应该能够转变成数据表示，而且不论怎么旋转、变化，数值表示应该不变。从graph获取表示的有chemical fingerprint, convolutional networks on graph, and similar graph-convolutions, or the bag-of-bonds approach. 3D几何的方法有Coulomb matrix approach, scattering transforms, atomic distances, etc.哪怕是最近在用神经网络的表示方法方法也不能将向量decode回分子。

提出了一个使用连续优化来产生新的化合物方法，通过建立molecule的数据驱动、向量表示。


# Methods

**Initial representation of molecules** 得先选择在encode和decode前后用什么离散形式表示molecules——SMILE，为了充分利用最近在文本建模很有效的序列对序列autoencoder。也尝试了用InChi表示，比SMILE稍微差点。

**Training an autoencoder** 训练一对recurrent neural network，这个encoder-decoder对就叫做autoencoder。autoencoder用来训练减少错误，比如[identify function](https://en.wikipedia.org/wiki/Identity_function)。这个autoencoder关键的一点就是要map到information bottleneck。这里bottleneck，一个固定的连续实数组成的向量，就是这个network学习到的向量，能够压缩表示抓住数据统计上最突出的特征。把这个vector-encoded molecule叫做latent representation。

SMILES的表示是一个个char，相对脆弱的内部句法（opening and closing cycles and brances, allowed valences）会导致无效的输出。我们用了开源的化学信息学组件RDKit来验证输出的molecule，并丢弃无效的。

为了使分子设计有效，连续数值表示的chemical structure需要和目标属性相关，因此在autoencoder的基础上，还训练了第三个模型，来预测分子的性质。为了提出新的candidate molecule，encoded之后的molecule的latent vector会按照提高attribute的方向走，然后这些新的vector会被decode。

**Autoencoder architecture** 先是使用的RNN，讲string encode到vector，然后decode到string，是sequence-to-sequence的对应。我们也尝试用了convolutional的方法，效果有所提升。

SMILES讲35个不同的字符进行编码，包括空格（讲几个short string连接起来）。我们会encode最多120个字符的string。VAE deep network的结构如下：encode用三个1D convolutional layers，filter size分别是9,9,1和9,9,10 convolutional kernels，紧随其后的是两个完全连接的layer，dimension是435和292.decode先是一个完全链接的layer，292，三个gated recurrent unit networks，hidden dimension都是501.

RNN decoder的最后一层，定义了一个所有字符在SMILES每个位置上的概率分布。这意味着最后的输出是有随机性的，同样的latent point可以map到不同的SMILES string，取决于用什么样的random seed。

**Bayesian optimization of molecules** 训练了一个稀疏的Gaussian process模型，有500个inducing points？？然后用了10步的Bayesian optimization，用expected improvement heuristic。每一次迭代，都选择一个50个vectors组成的batch，最大化EI。然后用了Kriging Believer Alg，就是在每次选择新的batch里的一个point，把这些data point作为新的inducing point在sparse GP模型，他们的associated target就是GP predictive distribution的平均值。当这样新选择了50个之后，batch里面的每一个点都被decode到SMILES，从SMILES string里，我们得到对应的数值。

# Results

**Using variational autoencoders to produce a compact representation** 为了保证从latent space都能decode到有意义的SMILES string，我们用了variational autoencoder(VAE)。VAE是的发展是作为一个解决latent-variable模型principled approximate-inference方法，每一个数据点都有一个对应、但是未知的表示。VAEs产生autoencoders，给encoder增加随机性，家penalty term。tuition就是通过给molecules增加早上，来时的decoder学会从更加多样性的latent points中decode。

# Appendix

[arXiv的链接](https://arxiv.org/abs/1610.02415)
