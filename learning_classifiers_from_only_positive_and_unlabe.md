# learning classifiers from only positive and unlabeled data

Charles Elkan, Keith Noto @ ucsd

# Abstract

仅从positive labels中随机选出一些，和unlabelled data进行训练。展示了两种方法，训练的classifier和真正的概率分布相差常数的因子。比目前最新的biased SVM方法要好。

# 1 Introduction

从nontraditional input中学一个traditional classifier，incomplete set of positive examples and a set of unlabeled examples。negative的例子有现实意义，比如biology中，有超过1000个专门的数据库，每一个都定义了一系列positive examples，比如包含在内的基因和蛋白质。但是不包含negative examples。一个database TCDB，给另外一个database SwissProt，问SwissProt中的蛋白质是否在TCDB内，这个就是有可能的。（其实这里有一个问题，为什么我们不能人工手动标记不在这个TCDB内的数据作为negative examples？等会儿和教授讨论一下）

# 2 Learning A Traditional Classifier From Nontraditional Input



# Appendix

文章是2008年14届SIGKDD发布的