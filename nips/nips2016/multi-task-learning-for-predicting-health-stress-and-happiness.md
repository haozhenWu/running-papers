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

通过一个共同训练的RFB kernel。

## Hierarchical Bayes with Dirichlet Process Priors (HBDPP)

## Neural Networks (NNs)

## Single task learning (STL) techniques