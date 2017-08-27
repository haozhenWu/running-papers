# Learning with Whom to Share in Multi-task Feature Learning

Zhuoliang Kang, Kristen Grauman, Fei Sha

# Intro

这篇文章考虑的是在学习一个shared feature的同时，决定哪些任务应该share参数。我们把这个问题抽象成一个mixed integer programming问题，能够确定group structures。

有两种主要的方法来确定task relatedness。第一个是假设所有的模型都类似，或者使用F-norm，或者共享某个common prior。另外一个给task relatedness建模的方法是假使所有的task都共享某个latent feature representation。

MTL有一个条件是，所有的task都indeed related，并且适合做joint training。当这个假设不成立的时候，negative transfer就会发生：dissimilar或者outlier task会导致更差的性能。negative transfer是一个很严重的问题，但是并没有太多人研究。Jacob在09年NIPS的会议上，提出使用clustering解决这类问题，而similarity就是task的参数。

在这篇论文里，我们也是在一个clustered MTL背景下，但是不同的是relatedness被当作是task之间shared features，来作为学习的目标。也就是让模型自己学习哪些task是相关的。



# Appendix

Jacob, Clustered multi-task learning: A convex formulation, NIPS 2009

实验的数据不是特别让人信服，不过可能也是当时那一批论文的特征。近期倒是可以看看十年前的论文，看看能不能有什么启发。