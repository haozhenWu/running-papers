# Visualizing and Understanding Recurrent Networks

Andrej

Justin Johnson

Li Fei-Fei

# Intro

RNN和它的变形LSTM最近很火。但是如此成功的原因和缺点还没有完全发现。

# Experimental Setup

## Recurrent Nueral Network Models

先介绍三种常见的RNN模型。$$h_t^l$$由$$h_{t-1}^l$$和$$h_t^{l-1}$$组成。

**Vanilla Recurrent Neural Network(RNN)**

$$h_t^l = tanh \, W^l \begin{bmatrix} h_t^{l-1} \\ h_{t-1}^l \end{bmatrix}$$

正向传播、反向传播算法一样。$$W^l$$在每一层都不一样，但是在不同的时间点，都是一样／共享的。

**Long Short-Term Memory(LSTM)** 简单的RNN会有一个问题，就是gradient会vanish或者explode。exploding gradient可以通过设置某些上限解决。LSTM就专门用来解决vanishing gradient的问题。LSTM有一个专门的门机制gating mechanism。

$$\begin{bmatrix} i \\ f \\ o \\ g \end{bmatrix} = \begin{bmatrix} sigm \\ sigm \\ sigm \\ sigm \end{bmatrix} \, W^l \, \begin{bmatrix} h_t^{l-1} \\ h_{t-1}^l \end{bmatrix} $$

$$c_t^l = f \bigodot c_{t-1}^l + i \bigodot g$$

$$h_t^l = o \bigodot tanh(c_t^l)$$

$$i, f, o \in R^n$$是三个二进制门。分别表示是否更新，是否设置为0，和是否本地状态表明hidden vector。$$g$$用于附加的修改memory内容。

**Gated Recurrent Unit(GRU)**

$$\begin{bmatrix} r \\ z \end{bmatrix} = \begin{bmatrix} sigm \\sigm \end{bmatrix} W^l_r \begin{bmatrix} h_t^{l-1} \\ h_{t-1}^l \end{bmatrix} $$

$$\tilde h_t^l = tanh(W_x^l h_t^{l-1} + W_g^l(r \bigodot h_{t-1}^l))$$

$$ h_t^l = (1-z) \bigodot h_{t-1}^l + z \bigodot \tilde h_t^l$$

## Character-Level Language Modeling

使用character-level作为测试。输入是一个字符序列，输出是预测每个时间点上，下一个最可能的字符，通过softmax。也就是，假设有K个字符，把所有的字符变成1-of-K的向量，然后把这个送给RNN进行训练，最上层得到了一个D维的hidden vector$$\{h_t^L\}$$，$$t=1,2...T$$。为了的到下一个字符的预测，用一个activation在最上层进行映射，把这个D维映射到K维的序列$$\{y_t\}$$，$$y_t = W_y h_t^L$$，而这个权重$$W_y$$是一个K*D维的矩阵。最小化的损失函数是平均cross-entropy。

## Optimization

用了前面研究的参数。

# Appendix

有[对应的PPT](http://www.robots.ox.ac.uk/~seminars/seminars/Extra/2015_07_06_AndrejKarpathy.pdf)。

这篇[CSDN博客](http://blog.csdn.net/Dark_Scope/article/details/47056361)介绍不错。

关于LSTM一篇[博客](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)。