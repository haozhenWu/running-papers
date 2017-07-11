# Why M Heads are Better than One: Training a Diverse Ensemble of Deep Networks

# Abstract

创建一个ensemble的最好方法是什么

# 1. Introduction

CNN提出之后，很多最新的模型都是基于CNN进行的ensemble方法。比如GoogLeNet。在这些方法中，用了多个分类器，然后他们的预测结果被用来产生一个更加平均和准确的预测。

+ Bayesian Model Averaging，ensemble是有限取样，来预测所有数据
+ Model Combination，
+ Reducing Estimation and Optimization Errors，减少base model的variance，通过目标函数的non－convexity、初始化和stochastic learning进行平均。

主要是发现了TreeNet能有效提高准确度，这种结构是几个CNN共享前面几层初始的layers。

# 3. Experimental Design

使用CIFAR10，CIFAR100，和ILSVRC2012。

使用ensemble－mean accuracy作为评估方式。另外也使用Oracle Accuracy，预测的方法是有一个“oracle”（神谕）选择最准确的ensemble成员作为预测结果。

# 4. Random Initialization and Bagging

这一节展示标准做法。

随机初始化network权重，和随机抽样（bagging）可能是最常用的方法。

四个模型，single model，random initial weights，bagging，和random initial weights+bagging。发现random initial weights的结果最好。

# 5. Parameter Sharing with TreeNets

TreeNet就是包含了0个或多个共享的初始层，后面紧跟着的是多个独立的层。训练的时候，初始层是每一个child network的graident加起来进行反向传播。

结果是这种方法比原本full train的ensemble方法提升，而且每一个branch都比上一节的方法准确度有提升。

# 6. Training Under Ensemble-Aware Losses

前面的两个章节中，每一个ensemble member都用同样的目标函数进行优化。先验证了设计同样的loss function有问题，然后给出了一个ensemble-aware loss function。