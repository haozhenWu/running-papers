# Learning Task Grouping and Overlap in Multi-Task Learning

Abhishek Kumar, Hal Daume

# Intro

multi-task learning作为一种transfer learning方法，经常会观测到negative transfer。这篇paper就提供了一个结构化的prior，能将关联的task形成group，并且不同的group直接会有一定的overlap。

已经有人做了类似的工作，Learning with Whom to Share in Multi-task Feature Learning, Kang ICML 2011，只不过他们用的是hard clustering，这篇论文用soft clustering。

# Learning Task Grouping and Overlap

假设是linear classifier，$$\hat y=W^T X$$，然后使用implicit factorization，假设$$W = L S$$。其中$$L \in \mathbb{R}^{d \times k}$$，表示有k个latent task，d是每一个task的feature dimension（也是data point的个数）；$$S \in \mathbb{R}^{k \times T}$$，是对应T个task，每一个latent task都能通过$$k \times 1$$的向量，linear组合成task representation。