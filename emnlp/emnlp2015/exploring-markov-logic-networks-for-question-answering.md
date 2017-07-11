# Exploring Markov Logic Networks for Question Answering

Tushar Khot,

Niranjan Balasubramanian

Eric Grifbkoff

Ashish Sabharwal

Peter Clark

Oren Etzioni

# Introduction

发明了一个系统，能够从书本里面获取信息，然后自动回答问题。自动提取信息会因为数据的不完整和噪声而受到影响。

Markov Logic Networks(MLNs)非常好地契合了这个应用。

MLNs模型的结果还是比不上word-based方法，因为输入信息的噪声和不完整性。但是通过增加一些条件，性能能大幅度提升。

# Background & Probabilistic Formulations

问题的构建就是，给定一个题干$$S$$，KB个规则，对于一个多项选择，有k个选项$$Q_i$$，要求解一个概率问题$$argmax_{i\in \{1,...,k\}} P[Q_i|S,KB]$$。

## 一阶(First-order)MLN（FOMLN）

对于一个由KN条一阶规则组成的集合$$R$$，自然地将QA任务转化成$$Pr[Q_i|S,R]$$。同时引入谓词，就是R中的规则，同时有一个二进制表示，蕴含了词汇黑盒的谓词。（也就是能够自动将意思相近的词汇连接在一起）

+ Evidence：在任意两个实体或者字符串之间，由蕴涵关系组成。
+ Query：查询的原子是$$result()$$。
+ Semantic Rules：除了KB条规则之外，我们还增加了语义规则，来获取我们谓词潜藏的含义，比如每一个任务要有唯一的agent。

缺点是计算效率比较低，因为对于任意一个问题，需要计算大量的grounded networks。

## Entity Resolution MLN（ERMLN）

定义的规则给予原型的实体或事件，而不是一阶变量。这些原型的实体或事件是被绑定到固定的常数的，词义相近的事件，其相似度更高。

引入了$$sameAs$$谓词。

由此可以看出，ERMLN会因为不同的词，但是根据映射到相同的实体而产生问题，比如两个不同的花，都映射到了实体”花“上。（因为是用聚类做的这么一个实体映射）

## Praline MLN

ERMLN缺乏灵活性，比如实体店映射上。所以为了解决，定义了一个MLN，给定KB条件，如何直接生成新的事实。也就是直接变成offline的问题。

+ Acyclic inference 无环推断：前面都是根据条件得出前提导致结果，但是没有保证是无环的。比如，万物依赖太阳，和太阳给万物提供资源，就可能产生loop。完善的QA会避免类似的问题。
+ False unless proven：除非被证明是真，否则都是假。

引入了新的谓词，holds。

# 总结

先引入了FO-MLN和ER-MLN，提出不足，最后综合有了Praline MLN。不过Praline MLN的解决方案还都是基于规则的。最后和word-based方法进行比较，有提升，但还是比不过。

# Appendix

这个paper坐下去还有很多有意思的想法。比如ERMLN的缺陷。（还是NLP的paper相对好懂啊～～～）