# Multi-task Learning for Predicting Health, Stress, and Happiness

Natasha Jaques, Sara Taylor, Ehimwenma Nosakhare, Akane Sano, Rosalind Picard

MIT Media Lab Affective Computing Group

# Intro

multi-task learning是一种transfer learning，模型能够同时训练多个相互关联的tasks。

# Dataset and Classification Problem

数据是自己收集，叫SNAPSHOT。收集了三十天，包含health，stress，和happiness，在早晨和晚上，标记的数值从0到100。

一共有343个特征，来自传感器，手机，调研和天气信息。MTL在这里有两种用法

1. 将每一个wellbeing标签当作task
2. 将每一个用户的wellbeing预测当作task

# Models

## Multi-Task Multi-Kernel Learning (MTMKL)

通过一个共同训练的RFB kernel。multi-task SVM。

## Hierarchical Bayes with Dirichlet Process Priors (HBDPP)

hierarchical bayesian learning使用一个共享的prior。这里使用了non-parametric Bayesian hierarchical模型来将相关用户进行聚类，同时进行MTL通过joint logistic regression。

## Neural Networks (NNs)

一个NN，共享前面几层layer的参数。

## Single task learning (STL) techniques

对应的三个模型，分别实现single-task版本。