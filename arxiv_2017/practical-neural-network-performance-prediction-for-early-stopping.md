# Practical Neural Network Performance Prediction for Early Stopping

Bowen Baker, Otkrist Gupta, Ramesh Raskar, Nikhil Naik

# Intro

目前有两个方法来减少neural network的设计：meta-modeling和hyperparameter optimization。前者是用来从空白直接设计NN的结构，而后者是在预先设计好的NN中进行搜索。两种方法都需要大量的计算。

sampling 不同NN configuration的时候，会考虑到很多低于标准的configuration。人类专家能够通过部分观测到的曲线来认识以及终止suboptimal model configuration。这篇paper，试着模仿人类专家的行为，并自动确定和终止subpar model configuration。我们提出了使用一个简单的SVM regression model，训练在部分的数据集，能够很好地预测validation accuracy。

# Related Work

# Background