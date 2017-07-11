# Atomic Convolutional Networks for Predicting Protein-Ligand Binding Affinity

Joseph Gomes, Bharath Ramsundar, Evan N. Feinberg, Vijay S. Pande

## Intro

atomic convolution 是受到了 atomic fingerprint neural network 的启发。论文链接在最后。与传统机器学习方法不同，不需要人工的输入数据，而是让模型自己来进行data-driven决策，从而对ligand binding进行预测。

这篇文章提出了使用Atomic Convolutional Neural Network(ACNN)，与CNN类似，只是采用了局部3D的convolution。而且以前的模型都是预测drug binding或者non-binding，ACNN直接预测binding free energy。

## Methods

**ACNN** ACNN引入了atom type convolution和radial pooling。

**Distance matrix** 使用笛卡尔距离。而且为了降低复杂度，对于每一个atom，只考虑M个邻居。并构成了一个邻居矩阵R。对应的还有一个atomic number matrix Z，每一个对应的是原子的类型。

**Atom type convolution** 使用R和Z。先使用一个$$1\times 1$$的stride，深度是类似于channel，总共的atom类型。

**Radial pooling layer** 用于降低维度，避免over-fitting。使用了radial filter。

**Atomistic fully connected network** 最后通过fully connected layer输出。对于每一个atom i，输出energy function $$E_i$$，那么整个molecule的energy function就是$$E = \sum E_i$$。

## Appendix

所以实际上并不是一个完整的3D CNN，还是在3D挖去特征，然后降维到2D，从而使用了2D CNN的方法。更加理想的方法应该是直接操作于3D结构。

Behler, Jörg, and Michele Parrinello. "Generalized neural-network representation of high-dimensional potential-energy surfaces." Physical review letters 98.14 (2007): 146401.

Behler, Jörg. "Atom-centered symmetry functions for constructing high-dimensional neural network potentials." The Journal of chemical physics 134.7 (2011): 074106.