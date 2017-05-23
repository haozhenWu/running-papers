# Improved Graph-based Dependency Parsing via Hierarchical LSTM Networks

Wenhui Wang, Baobao Chang

PKU

# Intro

这篇paper提出了graph-based dependency parsing，使用hierarchical LSTM的方法。

# Neural Network

首先是一个character-level Bidirectional LSTM来计算character-based word embedding。然后将四个向量embedding到一起：前向LSTM hidden vector，后巷LSTM hidden vector，word embedding $$e(w_i)$$，和POS tag embedding $$(e(p_i))$$。然后使用linear transformation $$w_e$$，和activation function g。

