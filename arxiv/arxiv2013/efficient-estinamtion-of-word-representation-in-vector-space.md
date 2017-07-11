# Efficient Estinamtion of Word Representation in Vector Space

Tomas Mikolov, Google

Kai Chen, Google

Greg Corrado, Google

Jeffery Dean, Google

## 1 Introduction

## 2 Model Architectures

### 2.1 Feedforward Neural Net Language Model(NNLM)

模型构建如下，首先是词库总共有V个，然后训练的时候输入是某一个单词的前N个1-of-V编码，输出是这个单词的1-of-V编码。具体如下

+ 输入层是$$N$$个单词的index。

+ 投影层是将每一个单词通过某一个$$|V|\times D$$维度矩阵映射到$$N \times D$$维度。

+ 隐藏层是$$H*1$$。

+ 输出层是$$V*1$$，表示每一个单词出现的概率。

### 2.2 Recurrent Neural Net Language Model(RNNLM)

RNNLM克服了NNLM的一些限制。比如不需要确定上下文的长度，而且RNN能够比浅层神经网络表达更复杂模式。RNN没有投影层。而且这类模型将隐藏层连接到recurrent矩阵，形成某种短期“记忆”。

### 2.3 Parallel Training of Neural Networks

模型训练是基于DistBelief，也就是TensorFlow的前身。

## 3 New Log-linear Models

CBOW模型就是给定上下文，预测单词。而Skip-gram就是给定单词，预测上下文。

### 3.1 Continuous Bag-of-Words Model

类似NNLM，不过去掉了隐藏层，并且映射层在单词之间进行共享。

换句话说，某一个单词的所有上下文单词向量，映射到一个单词向量上（通过取平均的方法），这一层叫映射层。这种方法是bag-of-words模型，因为单词的顺序并不会对结构产生影响。

### 3.2 Continuous Skip-gram Model

类似CBOW，但是与根据上下文预测当前单词不同，这个结果会最大化单词的分类，基于同一个句子内的另一个单词。增加结果单词的范围能够提高准确度，但也会增加运算复杂度。这个模型可以理解为是CBOW的输入和输出层倒置。

## 4 Result

发现skip-gram在语法上比CBOW效果略微差，但是在语义上比其他的模型好很多。

## Appendix

[Rachel Zhang的笔记](http://blog.csdn.net/abcjennifer/article/details/46397829)。