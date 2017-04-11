# Classes For Fast Maximum Entropy Training

Joshua Goodman, MSR

# Intro

这篇paper就是使用两层结构,对结果进行预测.首先就是判断这个单词是属于哪一个class,然后condition on class,预测这个单词.

先介绍了maximum entropy方法,如果使用它对模型进行建模,那么可以有

$$p(w|w_1 ... w_{i-1}) = \frac{exp(\sum_j \lambda_j f_j(w,w_1...w_{t-1}))}{Z_\lambda(w_1...w_{i-1})}$$.

其中$$ f_j(w,w_1...w_{t-1})$$是一个非常大的indicator function,比如t-1是"on",t-2是"meet",那么t="Wed"就是0.

至于$$\lambda$$是一个需要优化的参数.

# Appendix

这篇paper被收录于 ICASSP(International Conferences on Acoustic, Speech and Signal Processing) 2001