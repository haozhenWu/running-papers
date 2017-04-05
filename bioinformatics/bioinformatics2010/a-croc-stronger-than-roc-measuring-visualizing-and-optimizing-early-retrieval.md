# A CROC stronger than ROC: measuring, visualizing and optimizing early retrieval

# Intro

只对于prediction中排行最高的几个感兴趣，提出了CROC(Concentrated ROC)框架.比如一些经济因素,只考虑top 1%的预测.这叫做early retrieval problem.

# CROC Framework

## The ROC

ROC描述的是TPR关于FPR的函数.AC是TPR关于FDP的函数,其中FDP是给定某一个阈值有多少的数据被定义为active.

ROC和AC是线性关系.

## The CROC

