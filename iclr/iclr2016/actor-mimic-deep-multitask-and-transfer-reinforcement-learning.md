# Actor-Mimic Deep Multitask and Transfer Reinforcement Learning

Emilio Parisotto, Jimmy Ba, Ruslan Salakhutdinov

# Intro

DRL在高纬度的任务上展现比较惊人的成功。DQN最开始就是在基于像素的情况下，通过neural network，提供了一个parameterized方法，来表示state-action value function。

这篇论文的贡献就是发现并评估一个新的模型，这个模型能够实现DRL agent的multitask和transfer learning。

# Actor-Mimic

## Policy Regression Objective

假设已经有了训练好的N个STL model。我们想通过expert DQN来指导MTL的训练。一种方案是使用STL model的mse来作为指导。另外一个就是将Q-value转换为softmax。叫这个方法是Actor-Mimic，是因为policy就在模拟专家进行决策。首先是将每一个专家STL转换到一个policy network，由在Q-value上的Boltzman分布定义。然后将policy objective定义为，MTL和STL在每个任务上的cross-entropy和。

简单的说，就是把STL的结果当作oracle，或者是label；让我们的MTL来regress这个结果。然后用cross-entropy作为loss。

## Feature Regression Objective

使用DQN中的neural network来取feature，取的是最后一个activation来作为feature input。然后定义某个feature regression network，$$f_i$$，使用MTL的特征作为输入，输出是expert STL的特征。

## Actor-Mimic Objective

将上述两个regression求和即为Actor-Mimic objective

$$\mathcal{L}_{ActorMimic}^i(\theta, \theta_{f_i}) = \mathcal{L}_{policy}^i(\theta) + \beta * \mathcal{L}_{FeatureRegression}^i (\theta, \theta_{f_i})$$

从直观上进行理解：policy regression作为一个teacher (expert network)，告诉学生 (AMN) 如何act（模仿expert的行为）；feature regression objective作为老师，就告诉学生为什么要这么想（模仿expert思考的过程）。

## Transfering Knowledge: Actor-Mimic as Pretraining

将前面几层作为mapping to feature/latent space，新的任务训练就会现在related task上进行训练充当一个warm-start。

# Appendix

