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

提出一个data-driven的方法,同时考虑到本地语法信息和语料库级别的数据.首先使用text mining方法将文本划分到没有覆盖的段落,然后同时使用interestingness和diversity方法来过滤掉不重要的短语以及删除重复短语.

1. Candidate Phrase Mining:使用了数据驱动的方法,SegPhrasen(这个是另外一篇paper,要再看一下)用了distant supervision.
2. Salient Phrase Selection:通过找到candidate phrases算法,每一个文档都能被当作是phrases的集合，但大部分都不能给代表这篇文档.从两个不同的角度来找salient phrases：短语的interestingness和diversity.
    1. phrase interestingness:如果一个短语在整个语料库中不是经常出现，但是在这个文档中经常出现，那么就是salient phrases.测量的方式由论文中的$$r(\cdot)$$定义.
    2. phrase diversity：采用编辑距离作为两个词组直接的差异。也可以使用语义相似度策略，比如分布式相似度.

### Commonality and Distinction Measures

使用聚类的方法来找到'commonality clusters'和'distinction clusters'，会有类似无法确定聚类数目的问题。解决方法就是考虑到不同phrases之间的语义相关性.

Phrase Commonality：要求$$f(p,d)$$和$$f(p,d')$$同时很高.

Phrase Distinction：给定$$(d,d')$$，当满足以下条件时，phrase $$p$$有相对比较高的distinction score $$\prod(p,d|d')$$：$$f(p,d)$$比较高，相比于$$f(p,d')$$.

### Comparative Selection Optimization

如何得到phrase-document relevance score $$f(p,d)$$，以及如何得到phrase set $$\{ C, Q, Q' \}$$.



### An Efficient Algorithm