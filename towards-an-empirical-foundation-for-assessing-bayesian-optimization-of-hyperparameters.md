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



# Appendix

[这个ML组](http://aad.informatik.uni-freiburg.de/people/hutter/publications.html)最近几年做了很多和贝叶斯网络相关的内容.