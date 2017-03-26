# Summarizing Source Code using a Neural Attention Model

Srinivasan Iyer, Ioannis Konstas, Alvin Cheung, Luke Zettlemoyer

UW-Seattle

# Introduction

为了给代码自动生成总结。

自然语言生成natural language generation在传统方法中一直用来分开地决定说什么和怎么说。这样的方法在大范围应用中使用效果不太好。我们则相反，提出了端到端的neural network，叫CODE-NN，使用了LSTM和attention机制。

# Tasks

CODE-NN用于给定源代码，产生NL summary；还有给出NL summary，还原源代码。

# Dataset

数据来自StackOverflow。

# The Code-NN Model

使用attention机制，LSTM每次产生一个summary单词，组成了one-hot summary vector。这个向量中的每一个单词都是在给定的一个词库中选择。然后介绍了如何选取参数范围做tuning。

# Experiment ＆ Conclusion

对于GEN和RET各自选取了baselines和eval metrics。

# Appendix

感觉就是因为前人要么用来非ML的方法，要么就是用了ML的方法，但只用了attention机制。这里就是LSTM-RNN，加上attention机制，来从源代码和summary之间相互转换。