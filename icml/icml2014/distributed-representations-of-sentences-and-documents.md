# Distributed Representations of Sentences and Documents

Quoc Le, Google

Tomas Mikolov, Google

# Introduction

目前最常用的固定长度的特征是bag-of-words，但是有两个缺点：缺失了ordering，忽略了单词的语义。这篇paper引入了Parahraph Vector，非监督式的方法，从不定长的文本中学习定长的特征。

# Algorithm

## Learning Vector Represntation of Words

最常用的模型就是最大化平均log概率：

$$\frac{1}{T} \sum_{t=k}^{T-k} log \, p(w_t|w_{t-k},...,w_{t+k｝)$$

最后的预测用类似softmax的方法。

## Paragraph Vector: A distributed memory model

首先是每一个段落都会映射到向量，某个单词表示第i个段落就会用这第i个段落的向量。每个单词同样的映射到唯一的向量。

paragraph token可以被当作是另外的单词，用来记住当前上下文中遗漏了什么。所以这个模型也叫做Distributed Memory Model of Paragraph Vectors(PV-DM)。

最后训练得到的向量就能用来当其他模型的特征。

总结一下，这个算法分为两个步骤
1. 在已有的段落上，训练各种参数
2. (inference stage)在新的段落上，给段落的映射矩阵增加一列作为新的映射向量，然后来训练这个新的向量，而保持其他的参数不变。

## Paragraph Vector without word ordering: Distributed bag of words

上面的方法还是用的sliding window。另外一个方法就是忽略单词的上下文，而是让模型来根据输入的段落来预测单词。输入是一个paragraph映射之后的向量，而输出是随机选取的text window。这种方法叫做Distributed Bag of Words version of Paragraph Vector(PV-DBOW)。

# Appendix

有一个疑问是，为什么不在引入bag-of-words模型特征的时候，加一个特征，比如这个word在句子中的位置，这样是否能够保留其order关系？从而不造成语义上的损失？有待探究。