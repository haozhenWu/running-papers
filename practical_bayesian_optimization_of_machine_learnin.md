# Practical Bayesian Optimization of Machine Learning Algorithms

Jasper Snoek

Hugo Larochelle

# 1 Intro

ML模型中，parameter一直被当作是麻烦的事情。对于连续函数，Bayesian optimization假设未知的函数是从一个Gaussian process中取样，并且根据观测值，维护一个后验概率。或者，在我们的例子中，不同hyperparameter跑出来不同的测试结果。（这一句不是太清晰）

为了选择下一个hyper parameter set，可以选择在现在最佳结果的基础上优化expcted imrpovement(EI)或者是Gaussian process upper confidence bound（UCB）。EI和UCB被证明是许多寻找黑盒函数全局最优的有效方法。

ml算法和黑盒函数有许多不一样的特征。首先，ml需要的时间区分比较大：10个hidden－unit需要的时间比1000个hidden－unit少很多。其次ml通常会并行计算。这两种情况下，标准的GP优化不那么适用。

第一个贡献是使用GP kernel的Bayesian optimization。第二个贡献是新的考虑到变量数量和未知代价（比如时间）的算法。

# 2 Bayesian Optimization with Gaussian Process Priors

Bayesian optimization的目标是为了找$$f(x)$$的最小值。而Bayesian optimization是构造了一个概率图模型，然后根据这个图模型判断下面搜索哪里。搜索的方法是考虑到前一次评估和local gradient和Hessian。

执行Bayesian optimization需要两点。第一点，得选择一个函数的prior，我们选择了Gaussian process prior。第二得选择一个acquisition function。

## 2.1 Guassian Process

GP是一个方便有效的prior distribution。GP是任意点$$\{x_n\}_{n=1}^N$$都符合多维高斯分布。

## 2.2 Acquisition Functions for Bayesian Optimization

prior和$$\{x_n, y_n\}$$这些数据构成了posterior。acquisition function，就是一个点应该根据下一个优化参数预测为什么数值$$x_{next} = argmax_x a(x)$$。这些acquisition函数依赖于前面的观测值和GP参数。

# 3 Practical Considerations for Bayesian Optimization of Hyperparameters

尽管这个方法是一种优雅的处理方案，但是有很多限制。第一，选择covariance函数的参数不确定。第二，评估函数自身也有一定的好事。第三，优化算法应该能使用多核并行算法，来更好的适用于现在的计算环境。这一章节就针对每一个问题提出解决方案。

## 3.1 Convariance Functions and Treatment of Covariance Hyperparameters

GP能够表示丰富的分布就依赖于covariance函数。

## 3.2 Modeling Costs

提出了expected improvement per second。

## 3.3 Monte Carlo Acquisitions for Parallelizing Bayesian Optimization

已经有N个点完成了N此评估，还有J个等待评估。理想中的是，在所有的J个点中，根据acquisition函数选择一个新的点。这就是$$a(x)$$在J维的高斯分布下的期望，而这个J维的高斯分布的mean和covariance很容易得到。

# 4 Empirical Analyses


# Appendix

我逆序读到这两篇paper。这一篇和NIPS 2013 multi－task Bayesian optimization作者都有Jasper。


