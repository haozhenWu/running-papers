# Multi-task Learning with Deep Model Based Reinforcement Learning

Asier Mujika

Zurich, Switzerland

# Intro

使用RNN解决multi-task reinforcement learning。

使用Q-learning进行MTL会fail，这篇文章还讨论了为什么fail。提供了解决方案是Predictive Reinforcement Learning。

# DQN

以往的MTL都是每一个task训练一个expert network，而后有一个multi-task network将每一个expert的行为进行'copy'。也就是说，他并没有真正的学习Q-function，而只是将每一个task的function进行复制。

另外改变policy的幅度尽量小，否则大幅度的改变会引起Q-values变化过大，从而diverge slow down。对于类似ATARI已经有一个环境模拟器来说不要紧，但是在实际环境中，如何得到环境的observation和reward等是一个问题。

# Predictive Reinforcement Learning

将每一个time step进行featurize和labelize，从而进行supervised learning。

然后引入了一个residual unit作为RNN的hidden unit。如果公式写下来，大概是

$$h_{t+1} = f( \sigma_{normalize}(h_t); x_t)$$

# Appendix

抛开作者的写作能力不谈，这篇paper的想法，使用RRNN在RL上做MTL。等将来看了更多的paper再补充。
投了ICLR 2017，被拒。（竟然还有一个是strong rejection，ICLR的reviewer杀气重重啊 LOL）