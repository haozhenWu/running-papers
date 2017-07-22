# A Unified Perspective on Multi-Domain And Multi-Task Learning

Yongxin Yang, Timothy M. Hospedales

# Intro

我们提出了一个方法，能够替代zero shot learning：multi-task或者multi-domain是通过在不同的但是相近的任务或者领域之间共享知识。multi-domain指的是在不同的上下文domain中，对于同一个问题；multi-task指的是同一个domain解决不同问题。因为domain和task的区别很低，这里不做区别。

我们这片paper突出了一个核心概念是semantic descriptor。

相关的思路是zero-shot learning，我们的NN也解决了这个问题。而且还带来了一个新的问题：zero-shot domain adaptation，也就是给定descriptor的时候，将模型进行合成，从而适用于一个新的未见过的domain。

# Related Works

## Zero-Shot Learning

zero-shot learning（ZSL）就是在test task完全不知情的情况下，训练得到一个很好的semantic descriptor，从而能够在完全没有见过但是相关的task上有很好的表现。

# Model

考虑有M个domains/tasks，第i个domain有$$N_i$$个instances，第i个domain的第j个实例这么表示：$$\{ x_j^{(i)}, z^{(i)} \}$$，而特征就是$$y_j^{(i)}$$。在这个设定下，所有的实例都和semantic descriptor联系。

提出了一个two-sided neural network。左边是使用原始特征作为输入，进行representation learning的representation learning；右边是使用相对应的semantic descriptor z作为特征，进行的模型构造。

# Appendix

似乎也只是一个一拍脑门的想法，没有进行理论分析证明有效性。