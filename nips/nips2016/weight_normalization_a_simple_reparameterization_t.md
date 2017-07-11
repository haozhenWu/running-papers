# Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Network

Tim Salimans, Diederik P. Kingma

OpenAI

# 1 Intro

最近的深度学习研究表明了，根据一阶导数训练的神经网络，能够得到很好的结果，但是同时这个方法也高度依赖于优化的目标函数的curvature。

有一些方法用来提升cost gradient的conditioning。一种方法是乘以cost gradient用inverse Fisher information matrix来大致代替。这个approximate inverse可以通过
+ 对Fisher matrix进行Kronecker factored approx，然后求逆
+ 对inverse Fisher matrix进行Cholesky factorization
+ 将network的每一层输入whitening

或者不用preconditioning，而是改变模型的parameterization，来使得gradient更加贴近whitened natural gradients。比如batch normalization：将每一个neuron的输出根据mean和standard deviation进行normalize。这能够减少输出端covariate shift，也使得Fisher matrix更加贴近identity matrix。

conditioning的一个解释在[这里有](http://www.voidcn.com/blog/kangroger/article/p-6202810.html)，是函数输入变化时，对于输出变化的快慢。

还是沿着batch normalization的路子，这里提出来一个更加一般化的方法，weight normalization。


# 2 Weight Normalization

$$
y = \phi(w \cdot x + b)
$$
为了加快converge，提出了将parameter $$w$$放缩$$g$$，$$g$$是一个scalar。$$w = \frac{g}{\|v\|} v$$。

$$
\bigtriangledown_g L = \frac{\bigtriangledown_W L \cdot v}{\|v\|}, \bigtriangledown_v L = \frac{g}{\|v\|} \bigtriangledown_W L - \frac{g \bigtriangledown_g L}{\|v\|^2} v
$$
proof:

第一个很trival，第二个也差不多，其实核心就是求解$$\frac{\partial w}{\partial v}$$

$$
\frac{\partial \frac{g\cdot v}{\|v\|}}{\partial v} = \frac{g}{\|v\|} + \frac{g}{\|v\|} \frac{\partial \frac{1}{\|v\|}}{\partial v}
$$
又

$$
\frac { \partial \frac{1}{\sqrt{v_1^2 + ...}} } { \partial v_1 } = 
-\frac{1}{v_1^2 + ...} \cdot \frac{1}{2} \cdot \frac{1}{\sqrt{v_1^2+ ...}} \cdot 2 v_1 = \frac{v}{\|v\|^3}
$$
带入第一个式子，得

$$
\frac{\partial w}{\partial v} = \frac{g}{\|v\|} - \frac{v g v}{\|v\|^3}
=\frac{g}{\|v\|} - \frac{g \bigtriangledown_g L}{\|v\|^2} v
$$

证毕。

另外一种写gradient的方法是$$\bigtriangledown_vL = \frac{g}{\|v\|} M_w \bigtriangledown_w L$$,其中$$M_w = I - \frac{ww'}{\|w\|^2}$$.

应用几个式子，weight normalization达成了两件事情：一是将weight graident scale到了$$g/\|v\|$$，二是将graident的投影远离了weight vector。

## 2.2 Relation to Batch Normalization

$$
t' = \frac{t - \mu[t]}{\sigma[t]}
$$
其中$$\mu[t],\sigma[t]$$是前一个activation$$t=v \cdot x$$的平均值和方差。如果只有一层，那么batch normalization和weight normalization是一样的。


# Appendix

一些术语的解释

input features are whitened: independently distributed with zero mean and unit variance

conditioning的一个解释在[这里有](http://www.voidcn.com/blog/kangroger/article/p-6202810.html)，是函数输入变化时，对于输出变化的快慢。
















