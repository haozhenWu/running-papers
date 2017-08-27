# An Overview of Multi-Task Learning in Deep Neural Networks

Sebastian Ruder

# Intro

MTL的想法来自于在task之间共享representation，从而generalize better on original task。

**Motivation**： 从machine learning的角度，可以把transfer learning当作inductive learning(归纳学习)。可以通过增加inductive bias来帮助提升模型。而在MTL中，这个inductive bias就是有auxiliary tasks表示。

# Two MTL methods for Deep Learning

两个种类，hard 或者 soft parameter sharing。

**Hard parameter sharing**： 就是完全共享hidden layer，只是将output layer独立。hard parameter sharing能很大程度上降低overfitting的可能性。

**Soft parameter sharing**：每一个task有自己独立的模型和参数，不同模型的参数之间的距离用来当作regularizer。有用l-2 norm，也有用trace norm。

# MTL in non-neural models

MTL中最常用的两个想法：通过norm regularization实现的sparsity，和task之间相似性进行建模。

## Block-sparse regularization

没一个模型的特征进行向量化，维度为d，对于第t个model的$$a_t$$，所以T个model就变成了$$A \in \mathbb{R}^{d \times T}$$。许多算法都假设了sparsity，也就是所有的模型只共享一小部分特征。对应于matrix A，就表示只有几行不是0，而这几行就对应了所有的task都使用的特征。

为了实现这个目标，采用了l1 norm。对于STL，直接在对应的task上加一个l1 normalizer。对于MTL，在matrix A上进行。具体方法是，在每一行先计算l1 norm，这样就有一个d维的向量；然后针对这个d维的向量在计算l1 norm。更加一般化，就会使用mixed-norm constraint，比如$$l_1 / l_q$$ norm。这也叫做block-sparse regularization，因为matrix A的所有行都接近于0。

## Learning task relationships

前面提到的方法都是假设task相互之间是紧密联系的。但是并没有处理无关的情况，也就是negative transfer。

定义了三个函数，来计算每一个task model的性能，并结合起来作为一个constraint。也可以用来clustering的similarity function。

## Recent work on MTL for Deep Learning

**Deep Relationship Networks** Long and Wang，2015 的一篇论文。是在fully-connected layer上假设某种matrix prior。

**Fully-Adaptive Feature Sharing** 在迭代的过程中自动的寻找相关task。

**Cross-stitch Networks** 对于两个任务的情况。一开始是类似soft-sharing的结构，有两个独立的model。然后使用一个cross-stitch unit来运行模型确定一个task-specific network，利用前一层layer的参数，用linear combination来平衡另外一个任务的信息。

**Low supervision** NLP中，都是找到MTL的某种结构。比如entity recognition可以用来帮助speech tagging，那么前者就可以认为是后者的某种auxiliary tasks。

**A Joint Many-Task Model** 延续上面的说法，Hashimoto定义了一个task的结构。

**Weighting losses with uncertainty** 定义了某种不确定性uncertainty value，然后通过maximizing Gaussian likelihood，来挑战每一个task的相对权重。

**Tensor factorisation for MTL** 更多的目光开始放在了MTL在DL上的训练。Yang 2017将前面一直讨论的matrix factorization放到了每一层layer的tensor factorisation。

**Sluice Networks** 结合了hard parameter sharing, cross-stitch networks, block-sparse regularization, hierarchy（类似很多NLP任务）。

# Appendix

原文还有对应的[blog](http://ruder.io/multi-task/index.html)