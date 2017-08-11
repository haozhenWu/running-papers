# Self-Paced Multi-Task Learning

Changsheng Li, etc.

# Intro

有两种机制，来学习task之间的relatedness。

1. 假设所有的人物都共享low－rank feature representation
2. task的model parameters相互关联

这篇paper提出了在训练MTL的同时，考虑到了instance和task的复杂性。

# Self-Paced Multi-Task Learning

类似Learning Task Grouping and Overlap in Multi-task Learning这篇论文中的，使用线性模型和k个basis task。然后为了体现self-paced的想法，换一种regularizer。