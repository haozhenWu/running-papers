# Practical Neural Network Performance Prediction for Early Stopping

Bowen Baker, Otkrist Gupta, Ramesh Raskar, Nikhil Naik

# Intro

目前有两个方法来减少neural network的设计：meta-modeling和hyperparameter optimization。前者是用来从空白直接设计NN的结构，而后者是在预先设计好的NN中进行搜索。两种方法都需要大量的计算。

sampling 不同NN configuration的时候，会考虑到很多低于标准的configuration。人类专家能够通过部分观测到的曲线来认识以及终止suboptimal model configuration。这篇paper，试着模仿人类专家的行为，并自动确定和终止subpar model configuration。我们提出了使用一个简单的SVM regression model，训练在部分的数据集，能够很好地预测validation accuracy。

# Related Work

**Meta-modeling: ** 在NN的设定下，这个指的是设计NN结构的方法。最开始的meta-modeling是基于genetic algorithm。后来有了bayesian optimization和reinforcement learning。

**Hyperparameter Optimization:** hyperparamter 经常有learning rate, batch size。通过对network design space进行搜索。bayesian optimization也会用在这里，包括基于sequential model-based optimization (SMAC), Gaussian process (GP) 和 TPE。但是random search, grid search 在实际中是最常用的。

# Background

## MetaQNN

一种Q-learning方法。有一个Q-learning agent，从一个很大但是有限的空间中抽烟NN structure，通过从输入到输出层是不同的节点。通过agent trajectory定义的NN模型训练后，使用validation accuracy作为reward。但是每一个模型都需要训练大量的时间，T epochs，而T会很大来保证达到peak accuracy。

## Hyperband

使用hyperparameter sweeping。会迭代的去掉性能最差的模型，比如性能倒数百分之十的模型。

# Method

目标是为validation accuracy $$v(x,t)$$ 进行建模，其中x是NN configuration，t是epoch次数。每一个NN config x，训练了T次，都记录了validation set上的time series $$y(t) = y_1, y_2, ..., y_T$$。尝试n个不同的config，能够得到集合$$S = \{ (x^1, y^1(t)), (x^2, y^2(t)), ... (x^n, y^n(t)) \}$$。

然后不错的idea来了，我们对每一个hyperparameter set或者NN configuration进行featurization， 标记为$$u_x$$。同时获取time-series accuracies，$$y(t)_{1-\tau} = (y_t)_{t=1,2,...,\tau} $$是时间序列1到$$\tau$$的validation accuracy。所以为了预测$$y_\tau$$，使用特征$$\{u_x, y(t)_{1-\tau}\}$$。我们使用一个叫做$$\nu$$-Support Vector Regression方法来训练，follow up一个论文，在appendix中。

# Appendix

印证了一句话：ML世界构建模型是最重要的。这个使用ML来预测validation accuracy的方法很巧妙。（有种回到ACM看题解的感觉）

另follow up的paper： Bernhard Scholkopf, etc. New support vector algorithms.