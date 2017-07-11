# Analyzing Framing through the Casts of Characters in the News

Dallas Card, Justin H. Gross, Amber E. Boydstun, Noah A. Smith

## Abstract

提供一个发现并将latent "personas"（是实体的描述）聚类的未监督模型。我们的模型最终将文本按照实体的相似度进行聚类。我们在关于移民的新闻集合上进行了测试，并证明了personas能够帮助预测粗粒度的framing annotations。我们也引入了automated model selection作为鲁棒的特征评估。

## Introduction

社会科学中的一个重要概念，就是交流中必然会涉及到framing：选择观测到的现实的几个元素，并将强调彼此之间的联系来组装一个描述。

framing和几个NLP应用到的场景有联系：ideology,sentiment,stance。

framing能否被自动识别？已经有一些未监督式的框架分析，但大部分都是在单独维度上进行的。与将文章从自由到保守或者负面到正面连续放置不同，我们更在意发现基础广泛的模式。

这里，我们关注的是新闻里的故事，尤其是故事里的参与者。

我们的模型建立于DPM（Dirichlet persona model）。在DPM中，在短电影中未监督式发现“personas”，这是文字表达的特征混合：他们做了什么，被如何对待，和描述性的特征。

## Model Description

图一是plate diagram。模型的描述如下：

1. LDA中有K个主题。每一个模型都是V个word的多项式，从一个Dirichlet中获取。
2. 对每一个persona p和每一个句法关系种类r，在K个主题上定义多项式关系，每一个都是Dirichlet中获取。
3. 假设有无穷多的从一个基础分布得到的personas分布，每一个都是基于P个personas的多项式。
4. 对于每一个文档

DPM的区别是每一个文档有唯一的分布。

## clustering stories

DPM假设每一个文档有独立的分布。但是我们会期望某些类型的personas经常一起出现，比如关于法律的文章。因此我们会希望根据文档的personas进行聚类。为了实现这个，我们每一个文档的personas上增加了一个Dirichlet过程（DP）先验，使得聚类的数量可以按照文库的大小和复杂度。

尽管模型架设了有personas上无穷多的分布，但是DPs的性质是D个文档使用的数量会小于D。

## Dataset

MFC包含了大约4200篇从美国13个报社收集的移民的文章，从15个框架维度标注。每一篇文章都被标记为“primary frame”和一个整体的语调（比如支持、中立或者反对）。完整的框架维度在附件中。

为了把我们的模型在大集合的文章中进行训练，我们使用最开始获取时候的文章的分词。

## Identifying Entities

DPM原本的关注点是电影中的人物，通过entity recognition或者named matching。

## Appendix

[这里](http://www.keithto.ws/2007/03/nlp-frames-reframingfilters.html)有关于framing框架，reframing换框和filter过滤器的介绍。

另外一种简单的理解就是NLP中的框架是一种个人言行的潜台词。

