# Understanding Black-box Predictions via Influence Functions

Pang Wei Koh, Percy Liang

# Intro

以往理解deep neural network的方法是模型如何做出预测，比如通过将一个简单的模型只fit一下本地数据点，或者打乱测试数据看预测会怎么改变。

这篇论文反过来，从模型的预测反过来追踪训练数据。然后提出一个问题：如果没有这个数据，或者数据稍微改变一下会给模型带来什么改变。

使用influence function来解决这个问题，robust statistics的传统方法。

# Approach

**Upweighting a training point**

首先是如果将一个datapoint的权重增加很小的$$\epsilon$$，定义了influence of upweighting，在supplement中，推导并不复杂。

然后可以发现，如果要去除某一个datapoint，直接另$$\epsilon=-\frac{1}{n}$$即可。

下一步是利用chain rule测量upweighting z如何改变$$\theta$$。

**Perturbing a training input**

定义$$z_\delta = (x+\delta, y)$$。得到公式3。

如果$$x$$是连续的，而且$$\delta$$很小，可以进一步估算公式3。

$$\bigtriangledown_\theta L(z_\delta, \hat \theta) - \bigtriangledown_\theta L(z, \hat \theta) \approx [\bigtriangledown_x \bigtriangledown_\theta L(z, \hat \theta) ] \delta $$

# Efficiently Calculating Influence

计算influence主要是hessian需要特别大的计算量。另外还要对于所有的training set里的datapoint计算influence function。

两个问题都可以通过Hessian-vector product解决。

# Use of Influence Function

这里有提到了几个很有意思的用法。一个是用influence function来比较DNN和RBF判别器到底依据什么进行了判别。发现DNN是基于对数据的挖掘，而RBF更类似最近邻居。

在GAN类似的问题上，influence function也可以实现类似的功能：构造非常接近真实，而无法识别的image。

能够检测domain mismatch：training set和test set的分布不一样。这有可能发生在一些以时间作为cross validation的问题上，或者如论文中说的，不同医院的病人情况。这里测试了20k个病人的重新接纳情况，从100多个医院，127个特征。具体举了四个在test set中孩子的例子，使用influence function能够更加好的找到这四个孩子，并且最重要的几个特征都和孩子相关。

# Appendix

从理论到实验都非常不错的论文。ICML 2017 best paper。