# A CROC stronger than ROC: measuring, visualizing and optimizing early retrieval

# Intro

只对于prediction中排行最高的几个感兴趣，提出了CROC(Concentrated ROC)框架.比如一些经济因素,只考虑top 1%的预测.这叫做early retrieval problem.

# CROC Framework

## The ROC

ROC描述的是TPR关于FPR的函数.AC是TPR关于FDP的函数,其中FDP是给定某一个阈值有多少的数据被定义为active.

ROC和AC是线性关系.

## The CROC

核心想法是侧重于early retrival,使用某种映射,将原先的curve映射到侧重top ranking的另外curve.比如:

$$
f(x) = \frac{1 - e^{-\alpha x}}{1-e^{-\alpha}}, f(x) = x^{1/(\alpha+1)}, f(x) = \frac{log(1+\alpha x)}{log(1+\alpha)}
$$

其中的$$\alpha$$是magnification factor.对于以上的