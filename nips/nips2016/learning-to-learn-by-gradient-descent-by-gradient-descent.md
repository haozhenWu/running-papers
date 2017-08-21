# Learning to learn by gradient descent by gradient descent

Marcin Andrychowicz, etc

# Intro

现在已经成功的将手动挖掘的特征化为了自动学习特征。但是optimization algorithm还是人工设置的。这里提出了optimization design也可以当作一个learning问题。

传统的gradient descent只考虑了一阶导数，而没有考虑二阶导数。可以通过基于curvature info（比如hessian，Gauss-Newton，Fisher Info matrix）的rescale learning rate来实现。