# Comparative Document Analysis for Large Text Corpora

Xiang Ren, Yuanhua Lv, Kuansan Wang, Jiawei Han

## Intro

提出来一个新问题,CDA(Comparative Document Analysis),就是在一个大量文本的语料库中,发现两个文档之间的相同和不同.

CDA的主要任务:

1. 从每一个文档中提取salient phrase
2. 在文档对中发现共同点,通过找到和两个文档语义都相似的salient phrase
3. 找到不同点,还是通过salient phrase,只不过内容是互斥

以往比较文档的方法有这么几个挑战:

1. semantic commonality.以往只考虑了共同出现的单词,但是忽略了近义词,导致了很低的recall.
2. pairwise distinction.不同的短语应该将两篇文档联合起来看.现有的方法将每篇文档单独考虑.应该是,将两篇文档联合起来之后考虑,词组会不一样.
3. data sparsity.很多方法只是考虑了单词的overlap或者句子的repetition,来发现共同点.但如果只比较两篇文档的时候,overlap和repetition会很少发生.

使用以下几个思路解决

1. 考虑和两篇文档中的phrase构成近义词的phrase
2. 使用一个新的方法能够发现和一篇文档中的短语类似,但是和另外一篇的短语无关.
3. 为了解决数据离散的性质,不再使用文档内部的内容覆盖,而是从大量背景词库中挖掘phras-document co-occurrence.

## Model

先进行一番定义:$$\{C, Q, Q'\}$$分别由一个文档对document pair($$d$$和$$d'$$)中的salient phrase中选出来的common phrase $$C$$,文档d的distinct phrase $$Q$$和$$d'$$的distinct phrase $$Q'$$.

### Salient Phrase Generation

1. Candidate Phrase Mining
2. Salient Phrase Selection：通过找到candidate phrases算法，每一个文档都能被当作是phrases的集合，但大部分都不能给代表这篇文档。从两个不同的角度来找salient phrases：短语的interestingness和diversity。
    1. phrase interestingness：如果一个短语在整个语料库中不是经常出现，但是在这个文档中经常出现，那么就是salient phrases。测量的方式由论文中的$$r(\cdot)$$定义。
    2. phrase diversity：采用编辑距离作为两个词组直接的差异。也可以使用语义相似度策略，比如分布式相似度。

### Commonality and Distinction Measures

### Comparative Selection Optimization

### An Efficient Algorithm