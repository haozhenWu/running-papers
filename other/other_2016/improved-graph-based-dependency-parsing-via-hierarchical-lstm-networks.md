# Improved Graph-based Dependency Parsing via Hierarchical LSTM Networks

Wenhui Wang, Baobao Chang

PKU

# Intro

这篇paper提出了graph-based dependency parsing，使用hierarchical LSTM的方法。

# Neural Network

首先是一个character-level Bidirectional LSTM来计算character-based word embedding。然后将四个向量embedding到一起：前向LSTM hidden vector，后巷LSTM hidden vector，word embedding $$e(w_i)$$，和POS tag embedding $$(e(p_i))$$。然后使用linear transformation $$w_e$$，和activation function g。

然后将这个concatenated向量作为word-level LSTM的输入。用于enrich word vector representation。

# Score Model

neural network经常用来score dependency arcs。后面的一段描述不够详细，follow up中的一篇有更加详细的解释。

对于一个dependency pair $$(h,m)$$，是一个句子中的两个单词，head word h 和 modifier word m。使用word representation，h和m之间的distance，和segment embedding作为特征，其中distance feature embedding 随机初始化。每一个句子通过h和m分成了三个segment。然后在word representation上面通过一个forward LSTM，segment embedding能够通过LSTM hidden vectors直接进行减法计算。

所有的feature embedding都映射到了隐藏空间，使用direction-specific transformation来对edge direction建模

$$h = g(\sum_i W_{h_i}^d a_i + b_h^d)$$

其中$$a_i$$是feature embedding，$$W_{h_i}^d$$和$$b_h^d$$中的index $$d \in \{ 0, 1\}$$，表明head和modifier之间的方向。

输出层最终加到hidden layer的最上面作为scoring dependency arcs，

$$Score(h,m) = W_o^d h + b_o^d$$

# Appendix

一个从直觉上能解释的通，但论文中没有给定足够的理论支持。刚刚读完Omer Levy使用PMI matrix来解释embedding。个人觉得如果能有理论支持最好，否则还是偏把deep-learning当作黑箱操作的方法。

另外打算follow两篇paper

Improved transition-based parsing by modeling characters instead of words with lstms

Graph-based dependency parsing with bidirectional lstm