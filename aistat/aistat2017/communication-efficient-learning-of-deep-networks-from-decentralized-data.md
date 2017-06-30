# Communication-Efficient Learning of Deep Networks from Decentralized Data

H. Brendan McMahan, Eider Moore, Daniel Ramage, Seth Hampson, Blaise Aguera y Arcas

Google

# Intro

现在的算法都是在大量分布式的数据集上运算，这里提出了一个Federated Learning,一种去中心化的方法。

Federated Learning：有这么几个属性

1. 从实际世界的数据训练，相比于在数据中心的proxy data上训练，有更大的优势
2. 数据是隐私的，而且数据量很大
3. 对于监督式学习，标签可以通过用户的反应来推测

federated learning主要一点是考虑到了unbalanced和non-IID

# The FederatedAveraging Algorithm

