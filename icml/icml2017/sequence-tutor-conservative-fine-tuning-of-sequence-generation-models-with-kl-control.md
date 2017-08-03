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

$$L(\theta) = \mathbb{E}_\beta [ (r(s,a) + \gamma \, \underset{a'}{max} Q(s',a'; \theta^-) - Q(s,a;\theta))^2 ]$$

其中$$\beta$$是exploration policy，或者说是behavior policy，而$$\theta^-$$是target policy。说off－policy，是因为在训练的时候，只更新behavior policy，而固定target policy。