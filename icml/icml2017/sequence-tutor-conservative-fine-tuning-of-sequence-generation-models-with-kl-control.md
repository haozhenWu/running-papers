# Sequence Tutor: Conservative Fine-Tuning of Sequence Generation Models with KL-control

Natasha Jaques, Shixiang Gu, DzmitryBahdanau, JoseMiguelHernande-Lobato, Richard E. Turner, Douglas Eck

# Intro

使用RNN进行序列生成是比较常用的想法。这篇paper进行了改进：一个RNN先是通过在数据上进行MLE训练，得到了一个概率分布：给定序列的历史，生成下一个token的概率。另外一个RNN使用RL生成更高质量的输出。这里是使用了KL-control off-policy方法。

使用MLE进行训练RNN一直都有问题，比如在生成的序列上，noise会有叠加影响：前一个时刻的generated text shift，会影响到后面时刻。

这里提出了Sequence Tutor，通过task-specific reward，来使得RL和sequence generation RNN结合起来。通过将pre-trained RNN转换成Reward RNN来实现。不仅仅使用Reward RNN，还要使用KL-control中的想法，用KL-divergence作为penalize。

# Background

agent的目标是为了最小化sequence of action的reward，使用$$\gamma$$作为discount factor。最有的策略$$\pi^*$$符合Bellman optimality equation：

$$
Q(s_t, a_t; \pi^*) = r(s_t, a_t) + \gamma \mathbb{E}_{p(s_{t+1}|s_t,a_t)} [\underset{a_{t+1}}{max} \, Q(s_{t+1}, a_{t+1}; \pi^*)]
$$

其中$$Q^\pi(s_t, a_t) = \mathbb{E}_\pi[\sum_{t'=t}^\infty \gamma^{t'-t}r(s_{t'}, a_{t'})]$$是policy的Q function。在Deep Q-Network中，就是使用下面的loss function来估计Q function：

$$
L(\theta) = \mathbb{E}_\beta [ (r(s,a) + \gamma \, \underset{a'}{max} Q(s',a'; \theta^-) - Q(s,a;\theta))^2 ]
$$

其中$$\beta$$是exploration policy，或者说是behavior policy，而$$\theta^-$$是target policy。说off－policy，是因为在训练的时候，只更新behavior policy，而固定target policy。

# Sequence Tutor

给定已经训练好的sequence generation RNN，把pre-trained model当作黑箱的prior policy。

一共有三种LSTM模型，图1展示了流程。Reward RNN在训练的时候是固定的，并且当作prior policy。

## Q-learning with log prior augmentation

在本来应有的reward上，再算上prior：

$$r(s,a) = log \, p(a|s) + r_T(a,s)/c$$

其中c是一个常数，控制task-specific reward的重要性。给定前面的DQN目标函数，和修改之后的reward function，和学习到的policy，可以有

$$
L(\theta) = \mathbb{E}_\beta [ ( log \, p(a|s) + r(s,a)/c + \gamma \, \underset{a'}{max} Q(s',a'; \theta^-) - Q(s,a;\theta))^2 ]\\
\pi_\theta(a|s) = \delta(a= arg \, \underset{a}{max} \, Q(s,a;\theta))
$$

如同最后一个式子展示的，DQN学习的是一个deterministic policy，对于sequence generator不理想，最佳的应该是stochastic。因此，在模型训练完之后，我们通过预测的Q-value，成指数级地进行抽样。

## KL Control for Sequence Generation

如果能把这个问题变成sequential decision-making problem，而生产的序列性质是由target reward决定的，那么这个问题可以被认为是non-Markovian system的KL control问题。KL control就是让agent最大化task reward的同时，还要最小化deviation from a prior policy。这里，我们把一个训练好的MLE sequence model当作prior policy，目标就是训练一个新的sequence model，来最大化一下reward，同时还要尽可能接近原来的MLE模型。

我们展示了，这样的KL control形式可以写作Q-learning的变形。



# Appendix

DQN的paper：Mnih et al. Playing atari with deep reinforcement learning