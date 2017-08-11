# Curriculum Learning of Multiple Tasks

Anastasia Pentina, Viktoriia Sharmanska, Christoph H. Lampert

# Intro

假设task之间的相似性由task parameter之间的euclidean distance决定。这里假设是使用的linear classifier。

以往的模型是假设所有的task都一样重要，然后偶尔有outlier，因此想要研究潜在的task relationship，从而避免negative transfer。

将euclidean distance进行regularization也是另外一种常用的方法，learner有很多task，但是只关注其中一个，剩余的都当作额外信息。

我们的方法就是将multi-task learning降解成了许多domain adaptation问题。由教育领域启发，学习是一个顺序的过程。通过将任务进行排序，逐渐学习更多的知识，并且重复利用前面的知识。

通过使用PAC-Bayesian theory证明了generalization bound依靠于data representation，并且这篇论文的算法能够解决它。

# Related Work

这篇论文的想法是给所有的task设定一个权重向量。

一种常见的方法是将latent task通过linear combination结合起来。

其余几个关于使用权重向量的做法，都需要domain knowledge。

以前关于sequence learning的想法，更多的是在single-task的情况下，如何将sample进行排序。这里就是multi-task的情况下，如何将task进行排序。Kumar在10年的NIPS中，提出的self-paced learning也是如何对sample进行排序，不过后来在17年的AAAI上就已经延伸到了multi-task设定。

# Method

