# Deterministic Policy Gradient Algorithms

David Silver, Guy Lever, Nicolas Heess, Thomas Degris, Daan Wierstra, Martin Riedmiller

# Intro

这篇paper考虑deterministic policy gradient，配合连续的动作空间。deterministic policy gradient有一个非常吸引人的特质：它是action-value function gradient的期望。并且展示了deterministic policy gradient比相对应的stochastic方法好很多。

当动作空间是连续的时候，RL通常使用policy gradient的方法。基本的想法是，使用一个概率分布来表示一个policy，$$\pi_\theta(a|s) = \mathbb{P}[a|s;\theta]$$，能够随机的根据state和参数，选取动作。policy gradient代表性的随机抽样policy，并且将policy的参数更新到reward更大的方向。

这篇论文相反考虑deterministic policy $$a=\mu_\theta(s)$$。证明了有效性和简洁性。并且也展示了deterministic policy gradient是stochastic在policy variance接近0的特殊情况。

中间还是使用了bootstrap的想法，设计一个off-policy算法，通过behavior policy进行随机选择action。基本思路还是通过stochastic behavior policy选择action，但是学的target policy是deterministic。使用deterministic policy gradient来获得一个off-policy actor-critic算法，来获取一个可导的估计值来估计action-value function，然后更新。（这里是off-policy的原因是，behavior policy和target policy不同，前者stochastic，后者deterministic）同时引入了compatible function approximation的概念，来保证/减少估计不会bias policy gradient。

在实验上，证明了deterministic off-policy gradient比stochastic gradient policy结果好，尤其是高维的任务。

# Background

除了一些通用的anotation之外，这里再注明一下。

stochastic policy中，$$\pi_\theta$$是任意一个参数为$$\theta$$的policy，而$$\pi_\theta(s_t,a_t)$$就是在时间t，状态为s的情况下，选择动作a的条件概率。然后agent给一个state，action和reward的trajectory，而返回是total discounted reward。value function是total discounted reward的期望。agent就为了找一个return最大的policy。

## Off-Policy Actor-Critic

actor是为了调整参数，critic是为了评估Q函数。

在off-policy的设定中，performance objective函数是特定为了改变target policy的value function（平均到behavior policy的state distribution）

$$ J_\beta(\pi_\theta) = \int_S \rho^\beta(s) V^\pi(s) ds = \int_S \int_A \rho^\beta(s) \pi_\theta(a|s) Q^\pi(s,a) da ds $$

之后对其求导，已经使用估计值，能有

$$ \bigtriangledown_\theta J_\beta(\pi_\theta) \approx \int_S \int_A \rho^\beta(s) \bigtriangledown_\theta \pi_\theta(a|s) Q^\pi(s,a) da ds = \mathbb{E}_{s \sim \rho^\beta ,a \sim \beta} \bigtriangledown_\theta log \pi_\theta (a|s) Q^\pi(s,a) $$

# Appendix

TBC