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

$$\mathcal{E}_\lambda\{\hat W, \hat \Theta\} = argmin \sum_t \mathcal{L} (y_t, f(X_t, w_t)) + P_\gamma(W, \Theta)$$

其中

$$\mathcal{L} (y_t, f(X_t, w_t)) = \frac{1}{N_t} \sum_i \ell (y_i^t, f(x_i^t, w_t))$$

$$\Theta$$是task之间共享的knowledge。我们假设$$P_\gamma(W, \Theta) = \sum_t P_\gamma(w_t, \Theta)$$，也就是separable。这就是给定了一个scoring function，判断当前task学习的难易。很多MTL问题都能归化到这个问题。比如Multitask Feature Learning, Regularized Multitask Learning, Multitask Learning with Manifold Regularization, Multitask Learning via Alternating Structure Optimization, Sparse Coding for Multitask Learning。

有了这些设定，可以将SPL框架沿用到MTL下，通过给每一个task设定权重

$$\mathcal{E}_\lambda\{\hat W, \hat \Theta\} = argmin \sum_t \frac{1}{N_t} \sum_i \tau_{ti} \ell (y_i^t, f(x_i^t, w_t))+ P_\gamma(W, \Theta) + \lambda r(\tau)$$

有两个问题

1. 没有充分利用knowledge shared among tasks
2. $$\tau$$随着instance数目增加而增加

为了解决这个问题，只考虑task-level的权重。

$$\mathcal{E}_\lambda\{\hat W, \hat \Theta\} = argmin \sum_t \tau_t [\mathcal{L} (y_t, f(X_t, w_t)) + P_\gamma(W, \Theta)] + \lambda r(\tau)$$

同样的道理，将task-level的residual和$$\lambda$$进行比较，来判断一个task是否简单。

## Motivating Examples

**Self-paced Mean Regularized Multitask Learning(spMMTL)** 假设所以的任务参数都接近于某一个固定的参数$$w_0$$， spMMTL学习 $$\tau$$ 通过每一个task参数和$$w_0$$的距离。但是要求提前知道$$w_0$$。

**Self-paced Multitask Feature Learning (spMTFL)** 学习的是不同相关task直接的feature representation $$D$$。同时还能学习到$$\tau$$来选择那些任务简单，那些人物复杂。

**Self-paced Multitask learning with Alternative Structure Optimization (spMTASO)** 

# Appendix

总的就是利用residual，进行判断，一个task是否简单，以及是否使用这个task进行gradient update。

然后在linear的情况下，loss function对于$$W$$和$$\tau$$都是convex，因此可以迭代的进行update。