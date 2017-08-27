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



# Appendix

原文还有对应的[blog](http://ruder.io/multi-task/index.html)