# Large-scale Multi-label Learning with Missing Labels

Hsiang-Fu Yu, Prateek Jain, Purushottam Kar, Inderjit S. Dhillon

UTA, MS

# Intro

multi-label分类问题有两个challenge

1. 如果有上百万的label，数据范围太大
2. 有missing data

这篇paper同时解决了这两个问题，使用了Empirical Risk Minimization （ERM）框架。

针对于问题1，常用的解决方案是label space reduction，也就是减少label-space的维度，通过random projection或者canonical correlation analysis(CCA) based projection。但这种投影的方法有局限。

这篇paper提出了一个更加直接的方法，将问题狗造成一个low-rank linear model的学习过程。$$y^{pred} = Z^T x$$，然后转换成传统的ERM问题，从而能够使用各类的loss function和regularization。

# Problem Formulation

对于每一个data point $$i$$，$$d_i^j = 0/1$$分别表示absent或者present。