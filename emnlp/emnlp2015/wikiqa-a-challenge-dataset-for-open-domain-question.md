# WIKIQA: A Challenge Dataset for Open-Domain Question Answering

Yi Yang, Georgia Institute of Technology

Wen-tau Yih, MSR

Christopher Meek, MSR

## Introduction

问答系统，有一种做法是单纯地判断多少词overlap，这个做法在实际场景中，会得到过分好动结果（尤其和其他复杂的方法相比）。

另外，也产生了另外一个挑战：answer triggering：不一定在给定的答案中有正确解，需要QA系统判断出来。

使用WIKIQA的数据集，从Bing中挑选查询的logs，每一个log都对应于一个wiki page，将wiki page中总结的段落当作可能答案，label通过crowdsourcing完成。大约有1／3的包含正确的答案。

## WIKIQA Dataset

描述了如何生成数据，以及和QASENT的比较。

选取"WH"问题，然后提取对应的wiki page的summary。

crowdsourcing阶段，先判断是否包含答案，否则全部pass，是就进一步问是哪个句子回答了问题。

## Experiments

使用了三种方法：

ICLR，Paragraph Vector（PV），CNN。

+ ICLR：一种答案选择的模型，利用了词汇语义特征，包括单词匹配、WordNet和向量空间的词汇语义模型。
+ PV-cnt：是question vector和answer vector的余弦相似度。然后一层logistic regression，接word count。
+ CNN-cnt：使用word2vec作为输入，average-pooling。然后一层logistic regression，接word count。

# Appendix

很有意思的数据集。最后也强调了，主要是用来提出answer triggering这个问题。而且，相对应的，