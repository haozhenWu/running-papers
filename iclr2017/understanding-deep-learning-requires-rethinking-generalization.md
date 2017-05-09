# Understanding Deep Learning Requires Rethinking Generalization

Chiyuan Zhang, MIT

Samy Bengio, Google Brain

Moritz Hardy, Google Brain

Benjamin Recht, UCB

Oriol VInyals, DeepMind

# Intro

DNN模型,有一个特性是训练参数通常比训练样本多很多,而其中有些模型又能够呈现惊人的 低generalization error,也就是training error和test error差别很小.但也有些模型的generalize效果不好.

有很多统计的理论,使用了complexity measures,来解释了这个现象的原因.包括VC dimension,Rademacher Complexity,和uniform stability.

**Randomized tests**

一个中心发现是:DNN可以很容易的fit到随机label.更具体地说,neural network可以达到0training error,而test error则会有一个gap,因为training和test的label没有correlation.

**The role of explicit regularization**

明确的regularization作用:寻常的regularizer方法有weight decay,dropout,data augmentation.我们发现:明确的regularization可能提高generalization性能,但既没有必要,也不充分控制generalization error.

与经典的convex empirical risk minimization不同,(explicit regularization是去掉不必要项的必要手段),DNN中的regularization表演完全不同的角色.它更像是调节的参数,用于提高最终test error.

**Finite sample expressivity**

实验结果显示,越大的NN,越能fit各种各样的training data.我们的结果显示,哪怕只是两层的NN,也可以表示任意training data.

**The role of implicit regularization**

显性的regularizer,比如dropout和weight-decay,不一定是generalization的必要条件,但也不是说所有的模型在训练数据fit well的同时,能够在测试数据上fit well.SGD本身也是一个regularizer.

# Effective Capacity

我们的目标是为了验证NN的有效容量.使用non-parametric randomization test:选择一个NN的结构,同时在真实数据和随机过label的数据上进行测试;第二个测试是,在样例数据和label直接没有任何的关系.结果是几乎没有学习.直觉上,在训练过程中,我们应该能够看到训练不再converging或者急剧变慢.但我们发现,不同结构的NN都没有受到影响.

# Regularization

哪怕是有regularizer的情况下,对于随机的label,很多DNN模型还是能够很好的fit training data.如表2所示.但也有一些DNN模型效果变差.

而表1则展示了对于true label,不管是否使用regularizer,在training set上都能够很好的fit,但是test set的表现差距比较大.

根据各种比较,得出的结论是:regularizer可能和重要,但是通过改变模型的结构能够获得更高的准确度.

## Implicit Regularizations

early stopping是implicit regularization的一种方法. batch normalization在每一个mini-batch中,都进行normalize,很多模型中都被用到.

总结就是,不管是显性还是隐性的regularization,都不是使得模型更加generalize的原因.

# Finite-Sample Expressivity

以往的uniform convergence distribution,都是population level,也就是要求sample size n是DNN输入的多项式级别和深度的幂级别,现实中很难.

所以我们反过来分子finite-sample情况下,NN的表达性.我们会发现,只要参数p大于sample size n,那么NN就能完美表达这些sample代表的function.

# Implicit Regularization: An Appeal To Linear Models

在linear model中,$$y=Xw$$,如果$$X$$很'扁',那么就有无穷多解.通常判断不同local minima,我们使用curvature of loss function来判断优劣.但在线性的模型中,所有的optimal solution解释一样的.

所以Hessian无法判别时,可行的一种方法是从算法,SGD,的层次考虑.因为$$w_{t+1} = w_t - \eta_t e_t x_{i_t}$$,而$$w_0 = 0$$.所以我们有$$w = \sum_i^n \alpha_i x_i$$，也就是说,$$w = X^T \alpha$$,

所以最后可以得到$$XX^T\alpha = y$$,这有唯一解.

结果也验证了,使用这个方法方法,可以在test set上得到完美的结果.(测试MNIST)如果我们使用Gabor wavelet transform，那么可以使得运算在24核对workstation上,三分钟就有结果.

总结一下,所以能够完美fit数据的模型当中,SGD会converge到minimum norm的结果.而上述提出的kernel solution的norm会比SGD的结果大很多.