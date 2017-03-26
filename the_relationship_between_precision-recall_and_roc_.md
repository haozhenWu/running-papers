# The Relationship Between Precision-Recall and ROC Curves

Jesse Davis

Mark Goadrich

# Abstract

ROC经常用来评估binary classification,但是在dataset highly skewed情况下,不够准确.而precision-recall能够更加真实表示模型.

# 1. Introduction

现在大家在评估ml模型的时候,从acc转移到其他方法,因为尤其是对于计算概率的模型.Provost在1998年提出仅仅计算acc是不精确的,并提出了使用ROC作为评估方法.但是在data highly skewed的情况下,ROC会表现的过于乐观.Drummond和Holte提出了cost curve来解决这个问题.这篇paper讨论的是precision-recall.

precision-recall经常在信息挖掘中使用,是ROC一种替代方案.例子Fig 1给出了ROC和precision-recall的一组比较,在skewed的cancer数据的情况.ROC很好的情况下,precision-recall展示了这个模型还有很大的提升空间.

# 2. Review of ROC and Precision-Recall

在二分类问题上,按照真实label和预测的label,分为true positive,true negative,false positive,false negative.下面就是confusion matrix:

|   | actual positive | actual negative |
| ------| ------ | ------ |
| predicted positive | TP | FP |
| predicted negative | FN | TN |

在ROC空间内,x-axis是FPR,y-axis是TPR.$$FPR = \frac{FP}{FP+TN}$$, $$TPR = \frac{TP}{TP+FN}$$

PR空间内,则是x-axis是recall,也就是TPR,y-axis是precision. $$recall=TPR=\frac{TP}{TP+FN}$$,$$precision = \frac{TP}{TP+FP}$$.

# 3. Relationship between ROC Space and PR Space

**Theorem 3.1** 在recall不为0的时候,对于一个给定pos和neg数量的训练数据集,存在一个ROC和PR一对一的对应,两个curve包含了一致的confusion matrices信息.

**Theorem 3.2** 对于固定了pos个neg数目的训练集,一个curve A只有在PR空间内dominate curve B,才能在ROC空间内dominate curve B.

为了得到ROC曲线,通常使用了这个方法:先把所有预测的概率求出,递增排序.然后对于每一个i,如果$$class(i) \ne class(i+1)$$,并且$$prob(i) < prob(i+1)$$,构造一个分类器,调用每一个$$example_j$$,$$j \ge i+1$$.

这样ROC和PR中的每一个点都代表了一个分类器,而且每一个分类器都针对于某一个threshold:可以理解为把threshold以内的都当做0,大于threshold的都当做1.因此方法上,在test set上构建这个convex hull是错误的.为了解决这个问题,构造convex hull必须在tuning set上进行.首先使用上面的方法,但是是在tuning set上,找到candidate thresholds.然后在tuning set上构建convex hull.最后用这些在tuning set上选出来的thresholds,在test set上建立ROC或者PR curve.尽管这个并不能保证在test上是convex的,但保持了数据的分离性.

# 4 Interpolation and AUC

一个问题是如何连接空间内的任意两个点.AUC是两点之间的直线都能走到.但是PR不是.当recall改变时,precision并不是线性改变的.collary 3.1提出了一种解决方案,但是当有无穷多(很多)节点的时候,我们要考虑一种可行的.我们拓展了Goadrich在2004年的一个方案.

在PR space中,公式是给定两点,连接的应该是$$( \frac{TP_A + x}{Total \, Pos}, \frac{TP_A + x }{TP_A + x +FP_A + \frac{FP_B-FP_A}{TP_B-TP_A}x} )$$.

通常使用面积来进行表示,AUC-ROC使用Wilcoxon-Mann-Whitney statistic.按照上面的方法,引入在PR space中任意两点之间的点,我们就可以做出PR curve.(AUC-PR)

所有整个PR的算法是这样:首先找到ROC的convex hull.然后对应每一个candidate point,按照consufion matrix的对应在PR空间上找到对应的点.最终实现正确的interpolation画出PR曲线,求出结果.

# 5 Optimizing Area Under the Curve

用于优化AUC-ROC的算法不能用于优化AUC-PR.

# 6 Conclusion

4个贡献:

1. 对于任意一个数据集,ROC和PR的点都能对应
2. 当构建PR curve的时候,使用的点和ROC curve的点一样(类似)
3. linera interpolation在PR空间内是不可行的
4. 优化AUC-ROC的算法不一定保证能够优化AUC-PR.

# Appendix

很巧,两位作者都是UW Madison的前辈,advisor是David Page.这篇文章也是2006 ICML收录.

Jess Davis[wisc的主页](http://pages.cs.wisc.edu/~jdavis/),[现在的主页](https://people.cs.kuleuven.be/~jesse.davis/),post-doc是跟u washington的[PEDRO DOMINGOS](http://homes.cs.washington.edu/~pedrod/).

Mark Goadrich的[主页](http://mark.goadrich.com/).