# Hierarchical Probabilistic Neural Network Neural Language Model

Frederic Morin, Yoshua Bengio, U Montreal

先是分析了一下使用传统语言模型的复杂度,然后提出先将最后一层的输出，使用一个clustering，也就是两层预测，来代替,这样可以有效减少计算的复杂度.通过建设normalization的分母计算.

$$P(Y=y |X=x) = P(Y=y | C=c(y), X) \cdot P(C=c(y)|X=x)$$

第j层来预测第i个单词的概率是

$$P(v|w_{t-1}, ..., w_{t-n+1}) = \prod_{j=1}^m P( b_j(v) | b_1(v), ..., b_{j-1}(v), w_{t-1}, ..., w_{t-n+1} ) $$

