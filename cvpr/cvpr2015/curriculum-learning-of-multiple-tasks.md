# Curriculum Learning of Multiple Tasks

Anastasia Pentina, Viktoriia Sharmanska, Christoph H. Lampert

# Intro

假设task之间的相似性由task parameter之间的euclidean distance决定。这里假设是使用的linear classifier。

以往的模型是假设所有的task都一样重要，然后偶尔有outlier，因此想要研究潜在的task relationship，从而避免negative transfer。

将euclidean distance进行regularization也是另外一种常用的方法，learner有很多task，但是只关注其中一个，剩余的都当作额外信息。

我们的方法就是将multi-task learning降解成了许多domain adaptation问题。由教育领域启发，学习是一个线性的过程。