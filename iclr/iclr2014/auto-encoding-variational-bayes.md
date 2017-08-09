# Auto-Encoding Variational Bayes

Diederik P.Kingma, Max Welling

# Intro

variational Bayesian(VB)方法可以处理intractable posterior的approximation问题，使用optimization方法。但是，常用的mean-field方法需要对合理的posterior进行期望统计，这在大部分情况下也是无法实现的。我们展示了如何将variational lower bound进行reparameterization，来产生对于lower bound来说可导的unbiased estimator。这种SGVB(Stochastic Gradient Variational Bayes) estimator可以用来posterior inference，并且在对于使用SGD进行优化的算法来说很直接。

对于iid数据集和连续的latent variables，我们提出Auto-Encoding VB(AEVB)算法。在AEVB算法中，使用SGVB estimator来优化recognition model，从而不用使用类似MCMC这类计算了特别大的模型。当使用neural network当作recognition model，就得到了variational auto-encoder。

# Method

这一章节是为了得到有向图模型上的lower bound estimator。这里做了一些限制，在更一般化的情况（iid的数据集和连续的latent space），对global parameter inference使用MLE或者MAP，已经latent variable进行variational inference。

## Problem Scenario

假设$$X$$是iid的数据集，数据是有随机过程产生，包括一些没有观测到的连续随即数据$$z$$。随机过程包括两个部分：

1. 从某个prior distribution $$p_{\theta^*}(z)$$产生$$z$$
2. 从某些条件概率$$p_{\theta^*}(x|z)$$产生$$x$$

$$p_{\theta^*}(z)$$和$$p_{\theta^*}(x|z)$$都是来自于parametric families of distributions $$p_{\theta}(z)$$和$$p_{\theta}(x|z)$$。并且假设概率都是可导的，对于$$\theta, z$$。问题是$$\theta^*$$和$$z$$对我们都是未知的。

这里并不加税任何关于marginal或者posterior probability信息，这里更关心更加一般化的算法，能够在下面两个问题上有效：

1. Intractability： marginal likelihood的积分难以计算 $$p(x) = \int p(x|z) p(z) dz$$，和真实的posterior density难以计算 $$p(z|x) = \frac{p(x|z) p(z)}{p(x)}$$，所有VB算法的mean-field也难以计算。
2. Large dataset： 当数据量大，每一个batch optimization耗费太大，我们想在小数据集上进行parameter updates。

这篇paper解决了如下：

1. 对于$$\theta$$高效的ML或者MAP estimation
2. 对于隐藏变量 $$z$$高效的posterior inference
3. 对于变量$$x$$高效的marginal inference

为了解决这类问题，提出了recognition model $$q_\phi(z|x)$$，是对于真实posterior $$p_\theta(z|x)$$的估计。

## The SGVB estimator and AEVB algorithm

$$\mathcal{L}(\theta, \phi; x^{(i)}) = -D_{KL} (q_\phi(z|x^{(i)}) || p_\theta(x)) + \mathbb{E}_{q_\phi(z|x^{(i)})} [log \, p_\theta(x^{(i)}|z)] $$

其中 $$KL(q||p) = \int q(t) log \frac{q(t)}{p(t)} dt = \mathbb{E}_q (log \, q - log \, p) = \mathbb{E}_q [log \, q] - \mathbb{E}_q [log \, p]$$

## Reparameterization Trick

直接举例子，在univariate Gaussian的情况下，$$z \sim p(z|x) = \mathcal{N}(\mu, \sigma^2)$$，这时候可以假设$$\mu, \sigma$$已知，然后$$z=\mu + \sigma \cdot \epsilon$$，其中$$\epsilon \sim \mathcal{N}(0,1)$$，从而得到的函数对$$\epsilon$$可导。

# Appendix

[slides](http://dpkingma.com/wordpress/wp-content/uploads/2014/05/2014-03_talk_iclr.pdf)

这篇[CSDN博客](http://blog.csdn.net/u011534057/article/details/55045470)介绍很好

这里顺带做一些补充：

1. MLE和MAP

首先likelihood的定义为$$L(p_\theta(x)) = \prod_i p_\theta(x_i)$$

因为乘积难易计算，不妨取对数，只要能找到我们需要的$$\theta$$，从而有$$log \, L(p_\theta(x)) = \sum_i log \, p_\theta(x_i)$$

同时根据贝叶斯原理，我们有$$p(\theta|X) = \frac{p(\theta) p(X|\theta)}{p(X)} \propto p(\theta) p(X|\theta)$$

所以同样的道理，只要能找到我们需要的$$\theta$$，我们可以将目标进行再一次的转换

$$arg \, \underset{\theta}{max} \, [log \, p(\theta|X)] = arg \, \underset{\theta}{max} [ log\, p(\theta) + log \, p(X|\theta)]$$

这个就是MAP