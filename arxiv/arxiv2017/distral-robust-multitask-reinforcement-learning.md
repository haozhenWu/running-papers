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

# Appendix

有几篇相关paper

Razvan, etc. Actor-mimic: Deep multitask and transfer reinforcement learning