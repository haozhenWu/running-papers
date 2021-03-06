# Distral: Robust Multitask Reinforcement Learning

Yee Whye Teh, Victor Bapst, Wojciech Marian Czarneckit, John Quan, James Kirkpatrick, Raia Hadsell, Nicolas Heess, Razvan Pascanu

DeepMind, London, UK

# Intro

DQN中，使用DNN作为agent，但是训练需要大量的数据和时间，学习过程也可能是不稳定的。一个不同的方法是通过加入额外辅助的任务，和RL一起训练。

以前的multitask learning有这么几个弊端
+ gradient可能是negatively correlated，导致更加低效的训练
+ 不同的reward schema，很可能使得一个任务dominate其他任务

这里提出来了一个Distral的multitask训练方法，Distill & transfer learning。与共享参数不同，这里提出了使用“distilled” policy，来抓取不同任务之间的共同行为。

# Distral: Distill and Transfer Learning

Distral最核心的是一个shared policy，将task-specific policy中的共同行为和表达提取出来。然后关键步骤是，提取的distilled policy会通过使用KL divergence作为regularization，来指导task-specific policies。

## Mathematical framework

假设action space $$A$$和state space $$S$$对于所有的task都一样。但是转换$$p_i(s'|s,a)$$和reward function $$R_i(a,s)$$对于每一个task都不一样。$$\pi_i$$是task-specific stochastic policy。

目标函数是期望返回值加上policy regularization。policy regularizer是$$\gamma$$-discounted KL divergence: $$\mathbb{E}_{\pi_i} [\sum_{t \ge 0} \gamma^t log \frac{\pi_i(a_t|s_t)}{\pi_0(a_t|s_t)} ] $$

所以最终的objective function是

$$ J(\pi_0\{ \pi_i \}_{1}^n ) = \sum_i \mathbb{E}_{\pi_i}[ \sum_{t \ge 0} \gamma^t R_i(a_t, s_t) - c_{KL} \gamma^t log \frac{\pi_i(a_t|s_t)}{\pi_0(a_t|s_t)} - c_{Ent} \gamma^t log \,  \pi_i(a_t|s_t) ]  $$

$$ = \sum_i \mathbb{E}_{\pi_i}[ \sum_{t \ge 0} \gamma^t R_i(a_t, s_t) + \frac{\gamma^t \alpha}{\beta} log \, \pi_0(a_t|s_t) - \frac{\gamma^t}{\beta} log \, \pi_i(a_t|s_t) ]  $$

其中$$c_{KL}$$和$$c_{Ent}$$是regularizer的因子，$$\alpha = \frac{c_{KL}}{c_{KL}+c_{Ent}}$$, $$\beta = \frac{1}{c_{KL}+c_{Ent}}$$。$$log \, \pi_0(a_t|s_t)$$ is a reward shaping term, $$- log \, \pi_i(a_t|s_t)$$ encourages exploration。

## Soft Q-Learning and Distillation

上述的目标函数可以当作优化：在给定$$\pi_0$$情况下，对每一个$$\pi_i$$进行优化。也就是$$\pi_0$$固定时，每一个任务都可以当作独立的优化。可以使用soft Q-learning，也就是G learning优化：

$$
V_i(s_t) = \frac{1}{\beta} log \sum_{a_t} \pi_0^\alpha (a_t|s_t) exp [\beta Q_i (a_t,s_t)]\\
Q_i(a_t, s_t) = R_i(a_t, s_t) + \gamma \sum_{s_t} p_i(s_{t+1} | s_t, a_t) V_i (s_{t+1})
$$

# Algorithm

分别有KL divergence和entropy作为regularization情况。可以使用两种优化策略，alternating或者joint；前者同时训练distilled policy和task policy；后者类似EM，迭代的固定一个训练另外一个。对于每一个task训练的时候，也可以使用soft Q-learning代替policy gradient。soft Q-learning可能会训练的慢一点，但是训练的模型会更加稳定，尤其是非常不相同的task。

# Experiment

## Two room grid world

每一个task都是由不同的终点区分，每一个state都是由地图位置和前一次行为和reward表示。

## Complex tasks

在3D环境下的游戏做测试。

**Mazes** 不同的任务是不同的迷宫组成，随机的reward，和不同的目标。distral算法训练得更加快，并且更加稳定。

**Navigation** 和前一个任务不同，这里每一个episode（可以认为是一次完整的轮回）每一个task都有一样的地图，但是返回内容不一样。比如有的reward是随机的，有的要反复从不同的起始点出发，有的是有随机开闭的门。distral结果更优。

**Laser-tag** 需要agent学习标签自动程序。这里的结果distral并不是最好。

# Discussion

贡献是提出了一个distral RL方法，效果比纯粹的共享NN parameter方法的multitask要好。

另外观察到NN的参数没有什么具体的语法含义，所以预期从parameter space的角度进行regularization，不然从语义的角度进行regularization，比如policy。

# Appendix

有几篇相关paper

Razvan, etc. Actor-mimic: Deep multitask and transfer reinforcement learning