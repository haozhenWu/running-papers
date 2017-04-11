# Hierarchical Probabilistic Neural Network Neural Language Model

Frederic Morin, Yoshua Bengio, U Montreal

## Hierarchical Deconposition Can Provide Exponential Speed-Up

先是分析了一下使用传统语言模型的复杂度,然后提出先将最后一层的输出，使用一个clustering，也就是两层预测，来代替,这样可以有效减少计算的复杂度.通过建设normalization的分母计算.

$$P(Y=y |X=x) = P(Y=y | C=c(y), X) \cdot P(C=c(y)|X=x)$$

第j层来预测第i个单词的概率是

$$P(v|w_{t-1}, ..., w_{t-n+1}) = \prod_{j=1}^m P( b_j(v) | b_1(v), ..., b_{j-1}(v), w_{t-1}, ..., w_{t-n+1} ) $$

这就好比是一个树,每一个叶子是一个单词,每一个节点通过从根到节点的binary seq来index.比如可以构造一个balanced binary tree.如果再考虑到词频来构造树,那么更进一步提速三成.不过相比于考虑到词频,单纯使用balanced hierarchy的提速效果更加明显.

## Sharing Parameters Across the Hierarchy

如果对应于每一个节点,都构造一个分类器,除了存储上的消耗,还有overfitting的问题.因此要构造一个model,能够share parameters.

首先,word embedding可以当作一个全局已知量,也就是所有的node都能够访问.因为hierarcy中的每一个节点都假设有语义上的意义,所以可以将每一个节点连接到一个特征向量.可以把建模为$$P(b|node, w_{t-1}, ..., w_{t-n+1})$$,其中node是hierarchy中的到node的seq,b是该node的childrenbit,也就是下一个bit(0或者1).

## Using the WordNet to Build the Hierarchical Decomposition

模型中非常关键的点是如何构造hierarchical word clustering.这篇paper用了一些假设来保证单词embedding的唯一性和binary tree.

# Appendix

这篇paper是看目前最新的LSTM模型用到的

Character-Award Neural Language Models

算上这一篇

Hierarchical Probabilistic Neural Network Language Model

还有这一篇paper提到的

Classes For Fast Maximum EntropyTraining