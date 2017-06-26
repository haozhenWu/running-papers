# Atomic Convolutional Networks for Predicting Protein-Ligand Binding Affinity

Joseph Gomes, Bharath Ramsundar, Evan N. Feinberg, Vijay S. Pande

## Intro

atomic convolution 是受到了 atomic fingerprint neural network 的启发。论文链接在最后。与传统机器学习方法不同，不需要人工的输入数据，而是让模型自己来进行data-driven决策，从而对ligand binding进行预测。

这篇文章提出了使用Atomic Convolutional Neural Network(ACNN)，与CNN类似，只是采用了局部3D的convolution。而且以前的模型都是预测drug binding或者non-binding，ACNN直接预测binding free energy。

## Methods

**ACNN** ACNN引入了atom type convolution和radial pooling。

**Distance matrix** 使用笛卡尔距离。而且为了降低复杂度，对于每一个atom，只考虑M个邻居。

## Appendix

Behler, Jörg, and Michele Parrinello. "Generalized neural-network representation of high-dimensional potential-energy surfaces." Physical review letters 98.14 (2007): 146401.

Behler, Jörg. "Atom-centered symmetry functions for constructing high-dimensional neural network potentials." The Journal of chemical physics 134.7 (2011): 074106.