# Classes For Fast Maximum Entropy Training

Joshua Goodman, MSR

# Intro

这篇paper就是使用两层结构,对结果进行预测.首先就是判断这个单词是属于哪一个class,然后condition on class,预测这个单词.

先介绍了maximum entropy方法,如果使用它对模型进行建模,那么可以有

$$p(w|w_1 ... w_{i-1}) = \frac{exp(\sum_j \lambda_j f_j(w,w_1...w_{t-1}))}{Z_\lambda(w_1...w_{i-1})}$$.

其中$$ f_j(w,w_1...w_{t-1})$$是一个非常大的indicator function,比如t-1是"on",t-2是"meet",那么t="Wed"就是0.

至于$$\lambda$$是一个需要优化的参数.使用Generalized Iterative Scaling算法进行优化.因为这里的indicator function是一个非常大的函数，而且训练的时候使用的是枚举所有可能情况，也就是转换成了NPC问题，所以求解非常缓慢。

# Class-based Speedup

提速的方式就是前面提到的方法。每一个单词都分配到了唯一的cluster。decomposition的式子是

$$P(w|w_1,,,w_{i-1}) = P(class(w)|w_1 ... w_{i-1}) \cdot P(w|w_1...w_{i-1},class(w))$$

而且训练的两个分类器（分别对应于每一层的cluster）是可以分离的。给定上下文，预测类；给定上下文和类别，预测单词。

# Appendix

这篇paper被收录于 ICASSP(International Conferences on Acoustic, Speech and Signal Processing) 2001