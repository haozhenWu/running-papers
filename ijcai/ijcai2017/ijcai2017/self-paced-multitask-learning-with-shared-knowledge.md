# Self-Paced Multitask Learning with Shared Knowledge

Keerthiram Murugesan, Jaime Carbonell

CMU

# Intro

self-paced learning，是有教育学激发的学习方式。在教育学中，教授的课程并不是由老师预先设定，而是由学生动态的选择。就好比是先选择更加容易训练的instance，然后再训练难的instance。但简单地将self-paced learning延伸到multitask的领域，会使得学习参数过大，从而难以获得性能的提升。

一个问题是，不是所有的task都相等。难易有差别，而且可以有其他已经学习的模型来帮助解决。

我们提出了一个MTL的学习框架来解决前面提到的问题。他先是从简单的task set开始训练，然后逐渐引入更加复杂的任务，来构建shared knowledge base。我们提出的self-paced learning解决了三个问题

1. 将task selection嵌入到了模型选择中
2. 逐渐学习了shared knowledge
3. 一般化到更多的MTL训练上

# Background: Self-Paced Learning

self-paced learning能够先学习简单的模型，然后学习比较复杂的模型。我们假设是一个linear模型。self-paced learning的目标是:

$$\mathcal{E}_\lambda\{\hat w, \hat \tau\} = arg \, min \sum_i \tau_i \ell(y_i, f(x_i, w)) + \rho_\gamma(w) + \lambda r(\tau) $$

而后使用residual作为判断，判断$$\tau_i$$的重要性。见式子(3)。直观的解释就是，当residual比较小的时候，这个instance训练起来比较简单，就用于训练。

# Self-Paced Multitask Learning with Shared Knowledge

假设有T个任务。loss就是

$$\epsilon{L}_\lambda\{\hat W, \hat \Theta\} = argmin \sum_t \mathcal{L} (y_t, f(X_t, w_t)) + P_\gamma(W, \Theta)$$

其中

$$\mathcal{L} (y_t, f(X_t, w_t)) = \frac{1}{N_t} \sum_i \ell (y_i^t, f(x_i^t, w_t))$$