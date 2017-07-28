# Policy Gradient Methods for Reinforcement Learning with Function Approximation

Richard Sutton, David McAllester, Satinder Singh, Yishay Mansour

AT&T

# Intro

第一次将policy使用自己的函数估计值表示，与value function独立。并且展示了Williams的REINFORCE方法和actor-critic方法是这种方法的例子。

value function approximation有几个缺陷：
1. 找到的是deterministic policy
2. 对某一个action的value function只要有一点点小改变，就可能改变这个行为是否会被选中。比如Q-learning，Sarsa，DP对于很简单的MDP，都可能会收敛到不同的policy

以往都是估计value function，并用这个数值来得到/更新一个stochastic policy，我们提出了使用一个stochastic policy。比如，一个policy可以是使用一个NN表示，其中输入时状态，输出是每个action选择的概率，weight就是参数。然后就可以使用gradient descent的方法来更新NN。

# Policy Gradient Theorem

环境的动态表现为状态转移的概率$$P_{ss'}^a = Pr\{ s_{t+1}=s' | s_t=s, a_t=a \} $$，和期望reward $$R_s^a = E\{ r_{t+1} | s_t=s, a_t=a \} $$。agent每一个时刻的决策制定是policy，$$\pi(s,a,\theta) = Pr\{a_t=a|s_t=s,\theta \} $$，并且$$\theta$$是parameter向量，远远小于状态空间。

对于function approximation，有两种方法：
1. average reward formulation: 使用long-term expected reward per step来评估policy
$$ \rho(\pi) = \underset{n \to \infty}{lim} \, \frac{1}{n} \, \mathbb{E}[r_1 + r_2 + ... + r_n|\pi] = \sum_s d^\pi(s) \sum_a \pi(s,a) R_s^a $$
$$ Q^\pi(s,a) = \sum_{t=1} \mathbb{E}\{ r_t - \rho(\pi) | s_0=s, a_0=a,\pi \} $$
其中$$ d^\pi(s) = lim_{t \to \infty} Pr\{ s_t=s|s_0,\pi \} $$ 是在策略$$\pi$$下，状态的分布。
2. start-state formulation: 第二种是已经有了初始状态$$s_0$$，只关心从这个初始状态开始的long-term reward。
$$\rho(\pi) = \mathbb{E}\{ \sum_{t=1} \gamma^{t-1} r_t | s_0, \pi\}$$
$$ Q^\pi(s,a) = \mathbb{E} \{ \sum_{k=1} \gamma^{k-1} r_{t+k} | s_t=s, a_t=a, \pi \} $$
期中$$\gamma$$是discount rate。

# Appendix

可以配合David Silver的[讲义](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching_files/FA.pdf)理解。简单地说，value function approximation都是预测state/action function value，而这里我们预测的是action probability。第一个问题是，如果action space是连续的，那我们如何进行预测？通过regression方法输出连续的数值么？
