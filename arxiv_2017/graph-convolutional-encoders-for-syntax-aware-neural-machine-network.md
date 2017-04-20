# Graph Convolutional Encoders for Syntax-aware Neural Network Machine Translation

Joost Bastings, Ivan Titov, Wilker Aziz, Diego Marcheggiani, Khalil Sima'an

ILLC, University of Amsterdam

## Intro

以往的NMT Neural Machine Translation在语义表现上有缺陷，因为缺少能够从encoder中获取语义的简单有效方法。

前面已有的方法，或者不直接，或者在语义和翻译任务上有限制，这篇paper提出了给encoder一个能够提供丰富语义的方法，而且没有语义和翻译任务的限制。

attention-based NMT系统，将源句子表示成隐藏向量，然后通过这些向量翻译。而我们的任务就是能够将源句子中的每个单词周围邻居的语义信息考虑在内，并提供给encoder，从而提高翻译质量。既然单词是用向量表示，那么可以考虑使用依赖语义，依赖树，如图1所示，表示的是单词之间的关系。

使用graph-convolutional network产生单词的特征，也就是encoder。我们使用syntactic GCN，是构造与语义依赖树上的GCN。

## Background

### Neural Machine Translation

NMT的任务是给定一个翻译“对”，也就是两种平行的词库，来获取条件概率$$P(y|x)$$，NMT分为三个部分，encoder，decoder，和一个encoder转换到decoder的方式，比如attention mechanism。

#### Encoder



#### Decoder

### Graph Convolutional Networks

### Syntatic GCNs

## Graph Convolutional Encoders

# Appendix