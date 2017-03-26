# Optimization as A Model For Few-Shot Learning

Sachin Ravi, Hugo Larochelle, Twitter


# Intro

先讨论meta-learning.考虑k-shot, N-class的分类问题.对于每一个数据集(任务),训练集合由N个类别,每一个类别k个标记的样例组成,也就是training set有$$k \cdot N$$个样例.k设定为1或者5.

# Model

## Model Description

$$\theta_t = \theta_{t-1} - \alpha_t \bigtriangledown_{\theta_{t-1}} \mathcal{L}_t$$

其中$$\bigtriangledown_{\theta_{t-1}} \mathcal{L}_t$$就是loss function的梯度.

可以发现这个更新的公式和LSTM中的cell update类似

$$c_t = f_t \odot c_{t-1} + i_t \odot \tilde c_t$$

而其中$$f_t = 1, c_{t-1} = \theta_{t-1}, i_t = \alpha_t, \tilde c_t = - \bigtriangledown_{\theta_{t-1}} \mathcal{L}_t$$.

先从$$i_t$$开始考虑.$$i_t = \sigma [ W_I \cdot [ \bigtriangledown_{\theta_{t-1}} \mathcal{L}_t , \mathcal{L}_t, \theta_{t-1}, i_{t-1} ] + b_I ]$$

也就是learning rate $$i_t$$是基于当前参数$$\theta_{t-1}$$,当前梯度$$\bigtriangledown_{\theta_{t-1}} \mathcal{L}_t$$,当前损失$$\mathcal{L}_t$$,和前一步的learning rate $$i_{t-1}$$函数.有了这些信息，那么meta-learner就能够控制learning rate,并且迅速converge.

再考虑$$f_t$$,有可能最优的数值不是1.直觉上 如果当前在一个不太好的local minimum,使得learner参数收缩、忽略前一次的数值.这对应了损失函数很高,但是梯度靠近0.因此提出可以这么更新forget gate

$$f_t = \sigma [ W_F \cdot [ \bigtriangledown_{\theta_{t-1}} \mathcal{L}_t , \mathcal{L}_t, \theta_{t-1}, f_{t-1} ] + b_F ]$$

另外可以学习初始的cell state $$c_0$$，把它当作meta-learner的一个参数。

## Parameter Sharing and Preprocessing

提出了一个预处理的方法

+ 如果$$|x| \ge e^{-p}$$，则$$x = [ \frac{log|X|}{p}, sng(x) ]$$

+ 否则，$$x = [-1, e^p x)$$

这就相当于增加了特征的维度。

## Training

LSTM meta-learner的训练.

目标函数是针对于$$D \in D_{meta-train}$$,而目标函数是$$D$$中的$$D_{test}$$.每次在$$D_{train}$$上进行迭代,LSTM meta-learner从分类器接收到了$$(\bigtriangledown{\theta_{t-1}} \mathcal{L}_t, \mathcal{L}_t )$$,然后提出了新的参数$$\theta_t$$.

结合论文里面的Algorithm 1和Figure 2,可以理解为使用一个大的meta-learner,训练集是基于$$D_{meta-train}$$,而对于每一个'小'的数据集合,则按照以往正常的方法训练一个learner.

## Batch Normalization

Batch normalization是靠将batch内减去平均值,除以方差实现.在训练的时候,使用的是当前batch的平均值和方差,而在评估的时候,使用的是从训练集合来模拟测试集合的平均值和方差.这是对于普通的learner.

对于meta-learner batch normalization,解决方法是使用running average对于meta-learner的test.

## Appendix

直觉上感觉transfer-learning或者k-shot learning在输入样本的feature space是low dim的时候比较有效,而一旦在high dim空间,还是需要大量的输入样例.