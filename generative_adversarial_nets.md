# Generative Adversarial Nets

# Abstract

通过adversial process来评估generative method。同时训练两个模型，一个生成模型G，来抓取数据分布；一个判别模型D，来评估一个样例从training data产生的概率。

# 1 Introduction

Deep generative model与deep learning方法相比，影响较小，因为类似MLE的方法难以估计很多概率计算，并且很难衡量linear units的优势。我们提出了一个新的生成模型方法，解决了这些困难。

这里提出的adversarial net模型中，生成模型和判别模型进行对抗，这个判别模型是判断一个样例是来自模型分布产生还是数据分布。生成模型可以认为是伪造者，试图产生伪造的货币，并且不被检测到。而判别模型就类似警察，试着检测伪造的货币。这个对抗最终会使得伪造的结果类似真实的结果。

# 3 Adversarial nets

为了获取生成器的分布$$p_g$$，定义了输入噪声的先验$$p_z(z)$$，然后$$G(z;\theta_g)$$映射到数据空间，$$G$$是一个由multilayer感知器代表的可导函数，参数是$$\theta_g$$。第二个multilayer感知器是$$D(z;\theta_d)$$，输出是一个标量。$$D(x)$$表示$$x$$来自数据而不是$$p_g$$的概率。训练$$D$$来最大化正确的label，包括来自训练样例和$$G$$。我们同时训练$$G$$来最小化$$log(1-D(G(z)))$$。换句话说，$$D$$和$$G$$有如下minmax关系

$$\underset{G}{min} \, \underset{D}{max} \, V(D,G) = E_{x \sim P_{data}(x)}[logD(x)] + E_{z \sim p_z(z)} [log (1-D(G(z)))] $$

# 4 Theoretical Results

最主要的证明就是这个minmax的结果，就是生成数据的分布等于真实训练数据的分布。

# Appendix

[收录于NIPS 2014](http://papers.nips.cc/paper/5423-generative-adversarial)， 作者Ian是大牛Bengio的学生。

[这一篇介绍很好](https://chenrudan.github.io/blog/2016/11/12/gan.html)

对抗样本数是自己人为地添加一些对抗数据。对抗样本误分类是模型的线性性质导致。
对抗网络是另外一个概念。将生成器的输出给判别器判断是真实还是生成的，一旦真实和生成的分布有差异，（也就是判别器能够区分二者区别）那么生成器就微调参数。一直到判别器不能找到区别为止。