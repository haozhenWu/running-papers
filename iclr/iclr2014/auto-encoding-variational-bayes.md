# Auto-Encoding Variational Bayes

Diederik P.Kingma, Max Welling

# Intro

variational Bayesian(VB)方法可以处理intractable posterior的approximation问题，使用optimization方法。但是，常用的mean-field方法需要对合理的posterior进行期望统计，这在大部分情况下也是无法实现的。我们展示了如何将variational lower bound进行reparameterization，来产生对于lower bound来说可导的unbiased estimator。这种SGVB(Stochastic Gradient Variational Bayes) estimator可以用来posterior inference，并且在对于使用SGD进行优化的算法来说很直接。

对于iid数据集和连续的latent variables，我们提出Auto-Encoding VB(AEVB)算法。在AEVB算法中，使用SGVB estimator来优化recognition model。

# Appendix

[slides](http://dpkingma.com/wordpress/wp-content/uploads/2014/05/2014-03_talk_iclr.pdf)