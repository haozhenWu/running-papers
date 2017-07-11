# Segmental Recurrent Neural Networks

Lipeng Kong, Chris Dyer @ CMU

Noah Smith @ UW -Seattle

## Content

提出了segmental recurrent neural network。给定一个输入序列，定义一个输入序列segment上的联合概率分布和segment对应的label。

## Intro

考虑把输入序列分解到连续任意长度的segment和标记每一个segment的联合任务。对此提出的新方法是segmental recursive neural network(SRNN)，结合了machine learning两大特点：representation learning和structured prediction。

首先bidirectional recurrent neural network(RNNs)将每一个segment embed，这些embeddings就用来计算和label的紧密度。

同时SRNNs是semi-Markov conditional random fiels的变体，因为他是在给定输入序列的情况下，计算输出空间上的条件概率分布。

## Model

给定输入观测序列$$x= \langle x_1, x_2, ..., x_{|x|} \rangle$$，长度是$$|x|$$。SRNN定义了一个联合分布$$p(y,z|x)$$，这个联合分布是在一个标记好的序列，每个序列都有一段时间长$$z_i \in Z$$和一个label $$y_i \in Y$$。这些时间段duration $$z_i$$符合加起来就是整个序列，即$$\sum z_i = |x|$$。一个$$z_i$$代表一个时间段。

求解下面的式子

eq1: maximization

$$y^* = arg \, \underset{y}{max} \, p(y|x) = arg \, \underset{y}{max} \underset{z}{\sum} p(y,z|x) \approx arg \, \underset{y}{max} \, \underset{z}{max} \, p(y,z|x)$$

eq 2: marginalization

$$L = -log \, p(y|x) = \log \, \underset{z}{\sum} p(y,z|x)$$

其中 $$p(z,y|x) = \frac{1}{Z(x)} prod f(y_{i -k:i}, z_i, x)$$,。$$Z(x)$$是合适的normalization函数。为了保证$$f$$的表达性和计算效率，使用如下定义$$f$$的方式：

$$f(y_{i-k:i}, z_i, x_{s_i:s_i + z_i -1}) = w^T \phi( V[ G ] + a ) + b\\
G = g_y(y_i-k); ...; g_y(y_i); g_z(z_i); \overset{\to}{RNN}(c_{s{i}:s_i+z_i-1}; \overset{\leftarrow}{RNN}(c_{s{i}:s_i+z_i-1})
$$

$$\overset{\to}{RNN}(c_{s{i}:s_i+z_i-1}$$是计算forward segment embedding，将输入的$$z_i$$长度自$$s_i$$开始的序列进行编码。而$$\overset{\leftarrow}{RNN}(c_{s{i}:s_i+z_i-1})$$
就把序列的顺序反过来。$$g_y$$和$$g_z$$就将label candidate $$y$$和segmentation duration$$z$$映射到vector representation。除了使用neural network来计算本地的圈子，这个模型和semi-Markov conditional random field一样。

$$\overset{\to}{RNN}(c_{s{i}:s_i+z_i-1}$$和$$\overset{\leftarrow}{RNN}(c_{s{i}:s_i+z_i-1})$$的计算使用量bidirectional LSTM。

## Inference with DP

解决三个主要的inference问题。1 找到最合适的segmentation/labeling；2 评估划分函数$$Z(x)$$；3 计算后验边缘$$Z(x,y)$$。可以用动态规划解决优化提速。

## Appendix

提出了新的模型提升性能。

和Noah Smith一直提出的'derp learning'相一致，完全是偏向于black box。这个时候我们更多的应该是如何解释模型，而不是如何使用模型解决新的问题。