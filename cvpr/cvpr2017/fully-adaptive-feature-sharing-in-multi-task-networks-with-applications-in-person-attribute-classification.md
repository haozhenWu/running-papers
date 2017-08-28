# Fully-adaptive Feature Sharing in Multi-Task Networks with Applications in Person Attribute Classification

Yongxi Lu, Abhishek Kumar, Shuangfei Zhai, Yu Cheng, Tara Javidi, Rogerio Feris

# Intro

提出了一个新的MTL模型，能够实现 1)自动的学习MTL架构 2)主动选择在哪些任务之间共享信息。

在person attribute classification上进行，每一个人脸或者穿衣特征就当作一个task。

# Methodology

一般MTL每一层layer的width需要tune，这里提出的算法能够动态的找到MTL的适合的width。分为三个步骤

1. thin model initialization
2. adaptive model widening
3. training final model

**Thin model initialization:** 是VGG的thin版本，用来作为热启动。假设某一层的layer是$$l \times d$$，希望只使用$$d' < d$$行来进行更新，（也就是选组的过程）。本身这是一个NP问题，可以使用simultaneous orthogonal matching pursuit(SOMP)来求一个近似解。

**Top-Down Layer-wise Model Widening:** 是核心步骤。从output层往前，将layer进行group。关于similarity，我们会发现，如果一个样例对于一个task很容易，但是对于别的task很难，那么这两个task就是dissimilar。由此，可以将similarity这么定义：对于两个task，随机两个training data作为输入，同时观测到简单或困难的样本的概率。而这里简单或者困难的定义为样本true label和predicted label的差值，也可以认为是margin，或者residual。