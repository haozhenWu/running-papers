# Playing Atari with Deep Reinforcement Learning

Volodymyr Mnih, Koray Kavukcuoglu, David Silver, Alex Graves, Ioannis Antonoglou, Daan Wierstra, Matrin Riedmiller

DeepMind

# Intro

对于sensory data，如果使用DL的话，不用进行人工的feature representation，直接能够使用raw data and extract high-level features。

DL和RL有这么几个不同：
+ DL都使用人工标记的label和data，RL则是学习了一个scalar reward signal，通常是sparse，noisy，delayed。delayed是因为选择的action和收到的reward通常要在很多步之后才匹配到。
+ DL假设数据是相互独立的，而RL则是有连续关系的。
+ RL的data distribution/policy是变换的，而DL是假设有一个固定的data distribution。

这篇论文就证明了，将CNN和RL结合起来，能够克服这些问题。为了解决最后两个问题，correlated data和non-stationary distribution，用了relay mechanism，就是将历史数据进行打散，因此来使得training distribution over past behaviors更加平缓。

# Background

$$Q^*(s,a) = \mathbb{E}_{s' \sim \epsilon} [r + \gamma \underset{a'}{max} Q^*(s',a')|s,a] $$

$$L_i(\theta_i) = \mathbb{E}_{s,a\sim \rho(\cdot)} [ ( y_i - Q(s,a;\theta_i) )^2 ] $$

$$ \bigtriangledown_{\theta_i} L_i(\theta_i) = \mathbb{E}_{s,a\sim \rho(\cdot); s' \sim \epsilon } [ (r + \gamma \underset{a'}{max} Q^*(s',a') - Q(s,a;\theta_i) \bigtriangledown_{\theta_i} Q(s,a;\theta_i) ] $$