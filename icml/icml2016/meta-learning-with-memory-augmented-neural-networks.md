# Meta-Learning with Memory-Augmented Neural Networks

Adam Santoro, Sergey Bartunov, Matthew Botvinick, Daan Wierstra, Timothy Lillicrap

Google, SBOS

# Intro

包含memory的neural network可以用来提供一个有效的meta-learning方法。这里就提出了使用memory-augmented neural network(MANN)。

# Meta-Learning Task Methodology

对于meta-learning，我们学习一个参数能够减小expected cost，在某个数据的分布 $$p(D)$$ 上

$$\theta^* = argmin_\theta \, \mathbb{E}_{D \sim p(D)} [\mathcal{L}(D; \theta)] $$

在time step t的输入是$$x_t, y_{t-1}$$，也就是上一步的输入。这里的模型是为了预测 $$p(y_t | x_t, D_{1:t-1}; \theta)$$。