# Tutorial: Deep Reinforcement Learning

David Silver, Google DeepMind

# Reinforcement Learning

$$s$$：状态

$$a$$：行为

policy：决策，给定当前状态，决定当前行为，有两种

+ Deterministic policy：$$a = \pi (s)$$
+ Stochastic policy: $$a = \mathbb{P}(a|s)$$

value function：是将来行为／状态的reward

$$Q^{\pi}(s,a) = \mathbb{E}[r_{t+1} + \gamma r_{t+2} + \gamma^2 r_{t+3} + ... | s,a]$$

value function可以分解为Bellman equation:

$$Q^{\pi}(s,a) = \mathbb{E}_{s',a'} [ r + \gamma Q^{\pi}(s',a') |s,a ] $$

Bellman equation可以让我们很方便的使用DP方法来求解，因为这就是一个状态转移方程。可以通过穷尽搜索，找到所有状态，或者记忆化搜索，然后迭代地更新。

我们要选的就是optimal value function

$$Q^*(s,a) = \underset{\pi}{max} \, Q^{\pi}(s,a) = Q^{\pi^{*}}(s,a)$$

然后在更新的时候，我们可以有

$$\pi^{*}(s) = \underset{a}{arg \, max} \, Q^*(s,a)$$

# Approaches To Reinforcement Learning

+ Value-based RL
    + 估计optimal value function
    + 在任何policy下，能获得的最大value
+ Policy-based RL
    + 直接搜索最优的policy $$\pi^*$$
    + 这个policy会获取最大的future reward
+ Model-based RL
    + 通过环境构造一个模型
    + 让这个模型来进行决策

所以Deep RL就有相对应的三种方法，分别用来表示
+ Value function
+ policy
+ Model

# Value-Based Deep RL

## Q-Network

使用一个network来表示决策的分布

也就是输入为$$\langle s, a \rangle$$，Q-network表示为$$w$$，那么输出就是reward

$$ Q(s,a,w) \approx Q^*(s,a) $$

## Q-learning

给定一个状态和行为，来得到最优future reward

$$Q^{*}(s,a) = \mathbb{E}_{s'} [ r + \gamma \, \underset{a'}{max} \, Q(s',a')^{*} |s,a ] $$

将右侧 $$r + \gamma \, \underset{a'}{max} \, Q(s',a')^{*} $$当作目标函数

优化目标是$$I = ( r + \gamma \, \underset{a'}{max} \, Q(s',a')^{*} - Q(s,a,w) )^2$$

## Deep Q-Networks(DQN)

DQN是end-to-end learning。输出可以是$$\langle s, a \rangle$$，然后再转换成reward。

# Policy-Based Deep RL

## Deep Policy Networks

通过使用deep network，权重为$$u$$，来表示policy

$$ a = \pi(a|s,u) $$ 或者 $$a = \pi(s,u)$$

将目标函数表示为所有的reward

$$ L(u) = \mathbb{E}[r_1 + \gamma r_2 + \gamma^2 r_3 + ... | \pi(\cdot,u)] $$

通过SGD更新$$u$$，来获取更多的reward

## Policy Gradient

+ stochastic policy:

    $$\frac{\partial L(u)}{\partial u} = \mathbb{E}[ \frac{\partial log \pi(a|s,u)}{\partial u} Q^\pi(s,a)]$$
    
+ deterministic policy:

    $$\frac{\partial L(u)}{\partial u} = \mathbb{E}[ \frac{\partial Q^\pi(s,a)}{\partial a} \frac{\partial a}{\partial u}]$$

# Model-Based Deep RL

从环境中学习，主要挑战是如何准确地描述reward。

# Appendix

省略了很多细节，不过补充了几篇paper应该足够。

对于DP的理解比较容易，传统的Q-learning在给定所有action和state reward之后也比较容易计算。但是在未知的时候，如何结合Deep Learning进行一系列决策就比较有意思了。

Model-based Deep RL部分介绍比较简单，不过前面的已经可以尝试一下。

另外附上找到的一个[blog](https://kofzor.github.io/Reinforcement_Learning_101/#comparing-reinforcement-learning-algorithms)，很好的介绍了on-policy和off-policy的区别。