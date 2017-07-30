# Deterministic Policy Gradient Algorithms

David Silver, Guy Lever, Nicolas Heess, Thomas Degris, Daan Wierstra, Martin Riedmiller

# Intro

这篇paper考虑deterministic policy gradient，配合连续的动作空间。deterministic policy gradient有一个非常吸引人的特质：它是action-value function gradient的期望。并且展示了deterministic policy gradient比相对应的stochastic方法好很多。

当动作空间是连续的时候，RL通常使用policy gradient的方法。基本的想法是，使用一个概率分布来表示一个policy，$$\pi_\theta(a|s) = \mathbb{P}[a|s;\theta]$$，能够随机的根据state和参数，选取动作。policy gradient代表性的随机抽样policy，并且将policy的参数更新到reward更大的方向。

这篇论文相反考虑deterministic policy $$a=\mu_\theta(s)$$。证明了有效性和简洁性。并且也展示了deterministic policy gradient是stochastic在policy variance接近0的特殊情况。

中间还是使用了bootstrap的想法，设计一个off-policy算法，通过behavior policy进行随机选择action。基本思路还是通过stochastic behavior policy选择action，但是学的target policy是deterministic。使用deterministic policy gradient来获得一个off-policy actor-critic算法，来获取一个可导的估计值来估计action-value function，然后更新。（这里是off-policy的原因是，behavior policy和target policy不同，前者stochastic，后者deterministic）同时引入了compatible function approximation的概念，来保证/减少估计不会bias policy gradient。