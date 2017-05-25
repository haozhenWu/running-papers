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

# Appendix

一个从直觉上能解释的通，但论文中没有给定足够的理论支持。刚刚读完Omer Levy使用PMI matrix来解释embedding。个人觉得如果能有理论支持最好，否则还是偏把deep-learning当作黑箱操作的方法。

另外打算follow两篇paper

Improved transition-based parsing by modeling characters instead of words with lstms

Graph-based dependency parsing with bidirectional lstm