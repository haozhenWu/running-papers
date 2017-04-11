# Character-Aware Neural Language Models

Yoon Kim, Yacine Jernite, David Sontag, Alexander M. Rush

# Intro

language modeling是NLP中应用面很广的一个话题,可以用于语音识别,文本生成,和机器翻译.

传统的language modeling使用n阶段马尔可夫假设,基于计数的方法估计n-gram概率.训练简单,但是准确性低(因为数据有稀疏性).

Neural Language Model通过word embedding的方法解决了稀疏性,并且将embedding vector作为训练模型的输入.word embedding能够让语义上相近的单词,在向量空间上相近.NLM模型的缺点是无法获取subword information(比如语素,类似单复数,过去式).因此一些出现率比较低的词就无法充分的学习到.

这篇paper提出了使用character-level CNN来获取subword information,也就是不使用embedding作为NLM的输入,而是使用CNN的输出作为输入.而且与word embedding相比,模型的参数减少.

# Model

## Recurrent Neural Network

在每一个事件点t,RNN将输入$$x_t$$和隐藏状态$$h_{t-1}$$,来生成$$t_{t}$$:

$$h_t = f(W \cdot x_t + U \cdot h_{t-1} + b)$$

理论上,RNN在第t步,能够将所有的历史信息存储在隐藏状态$$h_t$$中.但实际上,vanilla LSTM很难存储长范围的信息,因为存在vanishing/exploding gradient问题,解决的方案是使用Jacobian's multiplication.

LSTM通过增加一个记忆单元,来学习长范围的依赖性.记忆单元能够减缓gradient vanishing问题,但是gradient explosion还是没有解决.RNN/LSTM当有多层的时候,性能更好.

## Recurernt Neural Network Language Model

词库的大小为$$\nu$$.语言模型就是给定前面$$[w_1, ..., w_{t-1}]$$,求$$w_t$$的概率分布.Recurrent Neural Network Language Model(RNN-LM)通过隐藏层使用softmax函数,来获取该概率.而输入就是embedding,$$X \in \mathbb{R}^{n \times |\nu|}$$.我们的模型使用character-level CNN.

训练的目标是最小化序列的negative log-likelihood(NLL)

$$NLL = - \sum_{t=1}^T log \, Pr(w_t | w_{1:t-1}) $$

## Character-level Convolutional Neural Networks

首先对character embedding.每一个单词都用都用character-embedding表示.如果传统的LSTM,就会直接对character进行hash,然后对word进行embedding.

