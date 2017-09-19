# On the Expressive Power of Deep Neural Networks

Maithra Raghu

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

