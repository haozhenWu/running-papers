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

# Deep Reinforcement Learning

DL使用raw inputs，配合lightweight updates，能够学习到更好的representation。

我们使用experience relay，在每一步都存储agent的经验，$$e_t = (s_t, a_t, s_{t+1}, a_{t+1})$$。每一个episode都是一系列的step。在进行训练的时候，将所有的episode和step打乱，抽样，进行训练。算法1描述了整个流程。

> 
+ 初始化relay memory $$D$$, 大小为 $$N$$
+ 初始化 action-value function，随机的weight
+ episode = 1 to $$M$$
    + 初始化序列$$s_1=\{x_1\}$$，并且preprocess $$\phi_1 = \phi(s_1)$$
    + for t = 1 to $$T$$
        + 有$$\epsilon$$的概率随机选一个action
        + 否则选择$$a_t = max_a Q^*(\phi(s_t), a; \theta)$$
        + 在模拟器执行行为a，并且得到观测到reward $$r_t$$和image $$x_{t+1}$$
        + 设置$$s_{t+1} = s_t, a_t, x_{t+1}$$，并且preprocess $$\phi_{t+1} = \phi(s_{t+1})$$
        + 将transition $$(\phi_t, a_t, r_t, \phi_{t+1})$$保存到$$D$$
        + 从D中，随机抽样一个transition minibatch $$(\phi_j, a_j, r_j, \phi_{j+1})$$
        + 如果$$\phi_{j+1}$$是终止，那么$$y_j=r_j$$
        + 否则,$$y_j = r_j + \gamma max_{a'} Q(\phi_{j+1}, a_j; \theta)$$
        + 进行一次gradient update
        
与online Q-learning相比，这个方法有这么几个优势：
1. 充分利用了数据，因为可以反复利用来更新模型
2. 使用了experience relay，避免使用连续的数据进行训练，从模型的角度，训练更加的高效
3. 在online learning中，看内层循环，刚刚更新到的模型会立即对下一个time step的取样产生影响；而通过使用expeirence relay，间接地起到了off-policy的作用，smoothing out learning让训练更加的平滑

# Appendix

