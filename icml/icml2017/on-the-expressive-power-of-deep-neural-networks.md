# On the Expressive Power of Deep Neural Networks

Maithra Raghu, Cornell, Google Brain

Ben Poole, Stanford

Jon Kleinberg, Cornell

Surya Gangli, Stanford

Jascha Sohl Dickstein, Google Brain

# Intro

在shallow network上，已经有很多研究从理论和实验证明了模型的表达性。最近一些研究depth特性的paper，也都考虑了模型的架构；但是只展示了如果对模型使用特定的weight，可以实现使用shallow network无法达到的性能。

这篇paper的目的是为了构造一个lower bound，基于手动设置的权重参数，这个lower bound在shallow和deep network有区别。

这篇paper的一个贡献是定义并且分析了一系列相互联系的measure和experssivity。

# Measures of Expressivity

给定一个network的参数A，$$F_A(x;W)$$是network的输出。我们想要研究随着A改变，$$F_A(x;W)$$会如何变化。

追踪整个input space在高维的特征情况下很难实现。这里先从1维开始。假设两个点$$x_0$$,$$x_1$$之间存在某一个trajectory $$x(t)$$。

有提到过使用linear region的概念。给定一个neural network和piecewise linear activation（比如ReLU和tanh），linear region计算的也是一个piecewise函数。一种测量experssive power的方法就是计算linear pieces/regions的个数，也决定了nonlinear function。

实际上linear region的改变是有neuron transition引起的。更加具体的说：

> 对于给定的参数，我们说一个包含piecewise linear region的neuron会在input $$x$$和$$x+\delta$$之间的变换，如果它的activation function在$$x$$和$$x+\delta$$之间的linear region。

对于更加一般化的trajectory定义有$$\tau(F_A(x(t); W))$$，来表示output neurons中的transition个数，当我们改变输入$$x(t)$$的时候。

如果我们把这个定义放到整个网络上，那么就有activation pattern。简单的理解，就是activation函数激活的个数。

比较shallow network和deep network，根据Zaslavsky's theorem，发现在parameter个数相同的情况下，shallow network的linear regions个数比deep的少。

**Trajectory Length**: 给定一个trajectory $$x(t)$$，它的长度定义为arc的长度

$$l(x(t)) = \int_t \| \frac{dx(t)}{dt} \| dt$$

然后证明了bound和stability。

# Insights from Network Expressivity

利用trajectory作为一个trajectory regularization。粗略一想也有l1 regularizer的味道。

说明了这么几件事情

1. 前面几层layer的pertubation，哪怕只发生了很小的改变，也会引起后续更深layer的改变，并且这种改变的关系是呈现exponential。
2. 可以用来解释batch normalization。实验证明它能够减少trajectory length，从而保证更高的stability。

# Appendix

