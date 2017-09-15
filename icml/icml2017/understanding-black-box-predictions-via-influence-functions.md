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

定义$$z_\delta = (x+\delta, y)$$。