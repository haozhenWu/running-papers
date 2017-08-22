# Learning to learn by gradient descent by gradient descent

Marcin Andrychowicz, etc

# Intro

现在已经成功的将手动挖掘的特征化为了自动学习特征。但是optimization algorithm还是人工设置的。这里提出了optimization design也可以当作一个learning问题。

传统的gradient descent只考虑了一阶导数，而没有考虑二阶导数。可以通过基于curvature info（比如hessian，Gauss-Newton，Fisher Info matrix）的rescale learning rate来实现。

很多update rules是基于domain，比如数据sparse时，考虑momentum，Rprop，Adagrad，RMSprop，ADAM。这篇文章提出了使用learned update rule来代替hand-designed update rules。

$$\theta_{t+1} = \theta_t + g_t(\bigtriangledown f(\theta_t), \phi)$$

## Related Work

有一些有意思的相关工作。

+ 学习一个base learner
+ 通过RL训练一个controller，来选择step-size
+ 使用parametric rules，而非SGD
+ 使用RNN学习step-size

# Learning to learn with recurrent neural networks

直接将optimizer参数化，目标函数是$$f$$，而optimizer 函数/参数是$$\phi$$，最终的优化目标是$$\theta^*$$。

使用RNN，输入的特征是$$\bigtriangledown_t, h_t, \phi$$，而输出是下一步RNN(m)的参数$$h_{t+1}$$和optimizer的参数$$g_t$$。之后使用$$\theta_{t+1} = \theta_t + g_t$$进行更新。

# Appendix

简单的说，就是让model能够学习gradient history，从而学习到下一步的gradient。