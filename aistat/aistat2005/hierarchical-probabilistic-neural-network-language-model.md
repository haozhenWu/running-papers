# Hierarchical Probabilistic Neural Network Neural Language Model

Frederic Morin, Yoshua Bengio, U Montreal

先是分析了一下使用传统语言模型的复杂度

然后提出先将最后一层的输出，使用一个clustering，也就是两层预测，来代替

这样可以有效减少计算的复杂度