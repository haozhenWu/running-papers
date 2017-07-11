# Massively Multilingual Word Embeddings

WAleed Ammar, George Mulcaire, Yulia Tsvetkov, Guilanume Lample, Dhris Dyer, Noah Smith

在一个共享的embedding space中，对embedding words进行评估。

## Estimating Multilingual Embeddings

希望对embedding进行评估
+ 同一种语言内，语义上类似的单词会比较近
+ 不同语言，但是含义一样的单词会比较近
+ domain包含尽量多的单词

使用distributional similarity来评估同一个语言内单词的相似度；至于不同的语言之间，或者使用parallel corpus $$P^{m,n}$$，或者使用bilingual dictionary $$D^{m,n}$$，这篇paper使用了后一个。

## Evaluating Multilingual Embeddings

关于单词的相似度，比如WordSim-353EN和MEM都是人工标记的语义相似度，但是仅限于来自同一个语言。

不同语言，可以采取word translation的方法。考虑一只language $$l_1$$的单词$$w_1$$与language $$l_2$$中的单词$$w_2$$。那么当$$cosine(E(l_1, w_1), (l_2, w_2)) \ge cosine(E(l_1, w_1), E(l_2, w_2')), \forall w_2' \in G_{l2}$$，embedding是1。其中$$G_{l_2}$$是language 2中包含的所有单词集合。

检测结果的评估使用QVEC-CCA。

QVEC：主要是衡量word embedding的语言学含义，通过最大化和一个人工标记的语言源的correlation。

$$QVEC = max \sum_{i=1}^{X} \sum_{j=1}^{S} r(x_i, s_j) \times a_{ij} $$

其中$$a_{ij} = 1 $$ iff $$x_i = s_j$$，$$r(x_i, s_j)$$是Pearson correlation。

QVEC-CCA：

为了评估embedding matrix $$X$$和linguistic matrix $$S$$，然后找到两个投影，使得投影过后的correlation最大。

$$QVEC-CCA = CCA(X^T,S^T) = max_{v,w} r(X^Tv, S^T w)$$

## Appendix

这篇的第二个部分得先挖坑后面再看了。太多NLP方面的东西需要补充。