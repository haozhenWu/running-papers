# Towards an Empirical Foundation for Assessing Bayesian Optimization of Hyperparameters

## Intro

以往很难在以前的benchmark上比较新的optimizer有两个原因

+ optimizers在不同的语言编写,并且用了不同的search space和文件格式.
+ hyperparameter optimization benchmark,与optimizer联合考虑的时候,通常封装性不好

但上述两点原因,个人感觉不是太大的问题,毕竟现在已经有TF,MXNet这类比较成熟的框架,上面还有类似Keras这类封装比较完好的库.

但这里作者还是收集了各类benchmark,并对三种贝叶斯优化方法进行了比较.

## Bayesian Optimization Methods for Hyperparameter Optimization

定义hyperparameter为$$\lambda_1, \lambda_2, ... , \lambda_n$$,那么hyperparameter space不妨定义为$$\Lambda_1, \Lambda_2, ..., \Lambda_n $$，对于每一个hyperparameter set $$\lambda \in \Lambda$$,用$$A_\lambda$$表示.用$$\mathcal{L}(A_\lambda, D_{train}, D_{test})$$表示损失,所以k-fold cross validation的平均损失函数就是

$$f(\lambda) = \frac{1}{k} \sum_i \mathcal{L}(A_\lambda, D_{train}^{(i)}, D_{test}^{(i)}) $$

贝叶斯优化是基于f的point evaluation和所有的prior info,构建一个probabilistic model $$M$$,并且使用这个模型使用下一个$$\lambda$$来评估.其中,贝叶斯优化使用acquisition function $$a_M:\Lambda \to R$$,是对模型参数预测其分布,来判断$$\lambda$$有多'有用',从而选择下一个$$\lambda$$.最常用的acquisition function是expected improvement.

$$E_M [I_{f_{min}}(\lambda)] = \int_{-\inf}^{f_{min}} max \{ f_{min} - f, 0 \} \cdot p_M(f|\lambda) df $$

现有的贝叶斯优化算法,主要区别是使用的模型类型.这里比较三个最出名的:

+ Spearmint.使用高斯过程Gaussian process(GP)来给$$p_M(f|\lambda)$$建模,并且对GP的hyperparameters进行slice sampling.
+ Sequential Model-based Algorithm Configuration(SMAC).使用随机森林给$$p_M(f|\lambda)$$来模拟Gaussian distribution,mean和variance是随机森林的mean和variance.
+ Tree Parzen Estimator(TPE).TPE没有直接给$$p_M(f|\lambda)$$建模,而是$$p(f < f*), p(\lambda | f < f *), p(\lambda | f \ge f*)$$建模,其中$$f*$$是一个固定的分位点.

# Hyperparameter Optimization Benchmarks

将benchmark分为三种,低中高维度的benchmarks.

# Appendix

[这个ML组](http://aad.informatik.uni-freiburg.de/people/hutter/publications.html)最近几年做了很多和贝叶斯网络相关的内容.

这篇论文主要贡献是将最近的贝叶斯优化的方法和benchmark进行了归纳.