# Self-Paced Learning for Latent Variable Models

M. Pawan Kumar, Ben Packer, Daphne Koller

Stanford

# Intro

latent variable model 这种算法，很容易卡在了bad local optimum。为了解决这个问题，提出了对输入的数据进行排序可以缓解。

最大的挑战是我们没有给定一个很好的方法来确定样本的“容易”程度。我们的结局方案是使用iterative self-paced learning algorithm，每次迭代的过程选择容易的样本，从而训练一个新的参数向量。

# Preliminaries

对于图像中识别汽车的任务，$$x$$表示图像，$$y$$表示是否有汽车的label，$$h$$是包含汽车的一个框架。之后通常是在数据上最大化likelihood，或者最小化risk。解决这个问题可以分为两个迭代过程，有两类算法：

**EM for Likelihood Maximization:** refer了这篇文章 Maximum likelihood from incomplete data via the EM algorithm

$$ \underset{w}{max} \sum_i log \, Pr(x_i, y_i; w) = \underset{w}{max} ( \sum_i log \, Pr(x_i, y_i, h_i; w) - log \, Pr(h_i|x_i, y_i; w) ) $$

**CCCP Algorithm for Risk Minimization:** 损失函数是

$$min \frac{1}{2} \|w\|^2 + \frac{C}{n} \sum_i \xi_i$$

# Self-Paced Learning for Latent Variable Models

两种方法判断easiness

1. 一个sample是easy，如果我们很相信hidden variable的价值
2. 一个sample是easy，如果很容易预测真的output

我们定义easy samples是output能够很容易的被预测。

$$w_{t+1} = \underset{w}{argmin} (r(w) + \sum_i f(x_i, y_i; w))$$

然后引入向量$$v$$来判断，第i个sample是否简单。只考虑简单的sample用于update weight。

$$(w_{t+1}, v_{t+1}) = \underset{w, v}{argmin} (r(w) + \sum_i f(x_i, y_i; w) - \frac{1}{K} \sum_i v_i)$$

$$f$$函数也就是residual，所以本质还是residual的比较。

# Appendix

