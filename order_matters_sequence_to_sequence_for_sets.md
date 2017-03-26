# Order Matters: Sequence To Sequence For Sets

Oriol Vinyals, Samy Bengio, Manjunath Kudlur

Google Brain

现在的deep neural network主要是解决了seq2seq的模型。这一篇解决了如果无法在自然语义条件下获得seq的情况下，怎么处理。比如排序或者联合概率。

# Neural Networks For Sequences and Sets

首先注意到的是，sequences可以被压成sets，只要把index作为特征就行。

# Input Sets

encoding或读取数据的时候，希望做到改变$$x_i$$和$$x_j$$的顺序，集合$$X$$的set encoding不会改变。

一种简单的做法是使用reduction操作，比如记录word count，或者word embedding，或者类似的embedding函数。但是有一个缺点是会使得representation低效：只能操作于固定的维度，而无法考虑到实际的长度。我们使用了attention mechanism来解决变长度的结构。

## Input Order Matters

原理上来说，RNN能够支持复杂的模型，顺序不一定那么重要。（也就是打乱顺序结构一样）我们认为实际中顺序影响结果的原因是，潜在的非凸包优化和更合适的prior。

第一个例子是机器翻译。有几个实验证明了，倒序的翻译结果会更好。另外，如果对数据进行预处理，通过对“点”的角度进行排序，任务会更简单（复杂度降低），模型训练更快更好。这些发现都表明了：对于优化，输入数据的顺序对于学习性能有影响。

最近提出来增加attention和computation的方法，能够让我们不用考虑顺序，而同时保持我们上面讨论的性质：一个memory可以随着集合的大小增加而增加，从而使得顺序无关。

## Attention Mechanisms

包含记忆单元的神经模型，加上可导的addressing mechanism，已经成功用于手写生成和识别、机器翻译和更一般化的计算机器。因为我们关心的是相关联的记忆，就引入了基于“上下文”的attention。这种attention机制有一个主要性质，就是从记忆中获取的向量并不会随着shuffle记忆而改变。公式如下：
$$

q_t = LSTM(q_{t-1}^*) \\

e_{i,t} = f(m_i, q_t) \\

a_{i,t} = \frac{exp(e_{i,t})}{\underset{j}{\sum} exp(e_{j,t})} \\

r_t = \underset{i}{\sum} a_{i,t} m_i \\

q_t^* = [q_t, r_t]

$$
改变$$m_i$$向量内部的顺序，并不会对结果造成影响。

## Read, Process, Write

模型分为三块：

+ reading block：将每一个$$x_i$$嵌入到RNN的memory向量$$m_i$$中
+ process block：就是一个没有输入和输出的LSTM，有针对于记忆$$m_i$$的T步计算。这个LSTM按照上面的公式，通过attention机制，对LSTM的状态进行更新。最后这个block执行完之后，$$q_T^*$$就是和输入$$x_i$$顺序无关的隐藏状态。
+ write block：是一个LSTM pointer network。输入是$$q_T^*$$，并指向$$m_i$$，一次一步。

# Sorting Experiment

在人工数据上进行了如下的测试：给N个没有排序随机的浮点数，返回排序后的结果。这是一个set2seq的例子。按照上述的架构，read模块是每一个数字的multi－layer perceptron。process模块是attention机制，是一个没有输入和输出的T步LSTM。最后是LSTM pointer network，产生输入数字的index。

# Output Sets

目前只考虑来encoding input sets的情况，现在考虑一下output representation。

## Output Order Matters

Y是一个序列或者集合。考虑Y的任意排列，然后为$$P(Y|X)$$条件概率分布进行建模，来证明输出的顺序对结果有影响。

benchmark是PennTree Bank。用三种顺序：natural，reverse，3－word。每个顺序训练不同的模型。

Vinyals提出了encode LSTM和decode LSTM一起构建了深度优先解析树。我们同时用深度优先和广度优先解析。深度优先的F1 score是89.5%，而广度优先则是81.5%，显示了顺序对结果的影响。

有一个问题是有许多等价的label。给定一个输入$$X$$，有$$n!$$种可能的输出，每一个都是有效的。那这样我们的映射结果就要考虑$$n!$$中排列组合，不是非常高效。前面的工作表明了，可以通过某些方法对输出的结果进行限制，能提高效率。

最后使用chain rule求联合概率。实验表明，当training size足够大的时候，不论顺序怎么变，LSTM都能学习到联合概率。否则，LSTM在optimal order情况下更容易学习。

# Appendix







