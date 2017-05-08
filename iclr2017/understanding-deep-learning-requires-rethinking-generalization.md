# Understanding Deep Learning Requires Rethinking Generalization

Chiyuan Zhang, MIT

Samy Bengio, Google Brain

Moritz Hardy, Google Brain

Benjamin Recht, UCB

Oriol VInyals, DeepMind

# Intro

DNN模型,有一个特性是训练参数通常比训练样本多很多,而其中有些模型又能够呈现惊人的 低generalization error,也就是training error和test error差别很小.但也有些模型的generalize效果不好.

有很多统计的理论,使用了complexity measures,来解释了这个现象的原因.包括VC dimension,Rademacher Complexity,和uniform stability.