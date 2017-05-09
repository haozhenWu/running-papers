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

# Effective Capacity of Neural Networks

