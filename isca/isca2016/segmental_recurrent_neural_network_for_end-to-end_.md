# Segmental Recurrent Neural Network for End-to-end Speech Recognition

Liang Lu & Steve Renals, University of Edinburgh

Lingpeng Kong & Chris Dyer, CMU

Noah Smith, UW-Seattle

# 1. Introduction

将CRF结合RNN，进行特征提取。

speech－recognition是标准的seq2seq问题。其中最重要的组成部分是acoustic model，能给定输入序列的情况下，给出输出序列的概率。然而计算这个概率会受到很多因素的影响，比如序列长度。HMM将这个seq-level的分类问题转换成了frame-level的分类问题。但HMM优缺点，就是它假设了独立性，和一介Markov规则。补充一个不错的Markov[材料](http://www.cnblogs.com/limitplus/archive/2010/03/05/1679451.html)。

CTC模型(connectionist temporal classification)直接定义损失函数来最大化输出序列的联合概率，但是还是需要输入和输出序列长度一样。CTC的解决方案是把实际label复制多份以及加空格。

attention-based RNN，它与CTC和HMMs不同的地方在于，它没有假设输入序列是conditional independent。在每个decoding步骤，都把变长的向量映射到定长大向量（比如softmax的几个类），然后由此产生output序列。[关于attention机制的一片论文](https://arxiv.org/abs/1409.0473)。但这个模型对于语音识别不能够非常自然地进行转换。

这篇paper我们讨论segmental RNN。和CTC和attention-based RNN很类似，因为也使用了RNN encoder进行特征提取，但是区别是sequence-level的联合概率是有segmental CRF定义的，而不是标准CRF。

# 2. Segmental Recurrent Neural Networks

$$

P(y,E|X) = \frac{1}{Z(X)} \prod exp \, f(y_j, e_j, X)\\

Z(X) = \underset{y,E}{\sum} \underset{j=1}{\overset{J}{\prod}} exp \, f(y_j, e_j, X)\\

f(y_j, e_j, X) = w^T \Phi(y_j, e_j, X)

$$
$$\Phi(\cdot)$$是使用RNN进行的特征表示，将声音信号和对应的label射到特征空间。

# 4. Conclusion

简单地说，segmental RNN就是将segmental CRF结合了encoder RNN，作为一种新的acoustic model。

# Appendix