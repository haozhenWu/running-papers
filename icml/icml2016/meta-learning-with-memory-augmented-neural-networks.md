# Meta-Learning with Memory-Augmented Neural Networks

Adam Santoro, Sergey Bartunov, Matthew Botvinick, Daan Wierstra, Timothy Lillicrap

Google, SBOS

# Intro

包含memory的neural network可以用来提供一个有效的meta-learning方法。这里就提出了使用memory-augmented neural network(MANN)。

# Meta-Learning Task Methodology

对于meta-learning，我们学习一个参数能够减小expected cost，在某个数据的分布 $$p(D)$$ 上

$$\theta^* = argmin_\theta \, \mathbb{E}_{D \sim p(D)} [\mathcal{L}(D; \theta)] $$

在time step t的输入是$$x_t, y_{t-1}$$，也就是上一步的输入。这里的模型是为了预测 $$p(y_t | x_t, D_{1:t-1}; \theta)$$。

# Memory-Augmented Model

使用一个controller来进行memory存储。controller也就是一个前向网络或者是LSTM。controller会和外部的一个memory module进行交互，而memory module能够存储重要的长期和短期记忆，在表现形式上就是能够将$$x_t$$作为输入，输出一个key $$k_t$$，key可以直接是memory matrix的某一项，或者是memory matrix的index。

使用基于cosine similarity将模型取出。是一种基于location和content的方法。

这个NTM的方法对于sequence-based task十分有效，但对于独立于序列的聚合信息不是特别最优。就此提出了Least Recently Used Access(LRUA)。大致想法就是将内容和访问次数联系起来，新的信息有两种加入memory的方式

1. 放到rarely-used location，保存recently encoded information
2. 放到last-used location，同时将memory内容更新，比如newer，more relevant info

而后就是考虑某种权重，将representation取出。

# Appendix

智能单元 [最前沿：百家争鸣的Meta Learning/Learning to learn](https://zhuanlan.zhihu.com/p/28639662)

> 我们可以看到，网络的输入把上一次的y label也作为输入，并且添加了external memory存储上一次的x输入，这使得下一次输入后进行反向传播时，可以让y label和x建立联系，使得之后的x能够通过外部记忆获取相关图像进行比对来实现更好的预测。