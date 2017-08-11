# Self-Paced Multi-Task Learning

Changsheng Li, etc.

# Intro

有两种机制，来学习task之间的relatedness。

1. 假设所有的人物都共享low－rank feature representation
2. task的model parameters相互关联

这篇paper提出了在训练MTL的同时，考虑到了instance和task的复杂性。

# Self-Paced Multi-Task Learning

类似Learning Task Grouping and Overlap in Multi-task Learning这篇论文中的，使用线性模型和k个basis task。然后为了体现self-paced的想法，换一种regularizer。

# Experiment

使用了两个真实的数据集，OHSUMED和Isolet。

第一个是regressional dataset，有106个queries，每一个query当作一个task。

第二个是150个speaker，每一个人都将英文字母说两遍。每一个英文字母作为一个task，当作一个regression value。所有的用户分成五个大类，通过speaking similarity。