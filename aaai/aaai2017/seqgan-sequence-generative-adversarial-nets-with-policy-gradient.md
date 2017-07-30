# SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient

Lantao Yu, Weinan Zhang, Jun Wang, Yong Yu

# Intro

RNN经常用于sequence generation，而且通常是最大化log predictive likelihood。但是后来Bengio在2015年的paper中提出了MLE会带来exposure bias问题：模型迭代的生成序列，产生的下一个token可能从来没有在词库中出现过。为了解决这个问题，Bengio提出了使用scheduled sampling，生成模型在产生写下一个token的时候，输入的是合成的数据（观测到的token）作为prefix。但后来Huszar的paper证明了这个方法并没有从根本上解决问题。另外一个可能的解决方案是在整个生成的序列上面构造loss function，而不是每一次转换。

GAN模型也可以用来处理上面问题。但是将GAN用于seq generation有两个问题
1. GAN是为了产生连续的数据，对于discrete还有问题
2. GAN只有为整个序列产生score/loss，对于partially generated seq没有

通过这个办法解决：把sequence generation当作sequential decision making过程，生成模型就被当作RL中的agent，state是已经生成的token，action是下一个选什么token赖声川。为了给reward，我们使用discriminator来评估生成的序列，而用评估的反馈来指导generative model。为了解决输出是discrete时候，无法使得gradient传递给生成器，使用stochastic parameterized policy，因为policy空间是可导的（但其实这里有一个问题，在David 2014年的一篇paper中，已经证明其实deterministic off-policy也可以是可导的，或许将来可以尝试）。

# Sequence Generative Adversarial Nets

训练一个generative model $$G_\theta$$，策略是随机的。判别模型discriminative model $$D_\phi$$，是用来判断多大概率这个序列是真实数据还是人工生成的。同时使用policy gradient和蒙特卡洛搜索来更新$$G_\theta$$。其中MC收到的reward期望是来自于$$D_\phi$$判断为真是序列的likelihood。

## SeqGAN via Policy Gradient

生成模型的目的是为了返回从开始状态能生成最大reward期望的序列

$$J(\theta) = \mathbb{E}[R_T|s_0, \theta] = \sum_{y_1} G_\theta(y_1|s_0) \cdot Q_{D_\phi}^{G_\theta} (s_0, y_1) $$

一个问题是如何policy evaluation。考虑使用$$D_\phi$$给出的概率值作为reward。但是判别器只给finished sequence提供reward value。既然我们关心的是长期的reward，那么每一个timestep，不应该直关心前面已经生成序列的fitness，同时也要关心最终的outcome。因此，为了评估中间状态的action-state value，在MC搜索上再应用roll-out policy $$G_\beta$$，来抽样后面的tokens。如果rollout是non-uniform，就可以有$$\epsilon-greedy$$。，不过这里没有提具体哪种rollout。

Algorithm. 1描述了完整的流程。pre-train可以有效提训练效率。

## The Generative Model For Sequences

使用LSTM

## The Discriminative Model for Sequences

DNN,CNN,RCNN是常用的序列分类算法。这里使用CNN。

# Synthetic Data Experiments

测试是使用了一个随机的LSTM作为true model，来产生real data distribution。

## Evaluation Metric

使用人工合成的LSTM好处就是能够准确的评估性能。（evaluation在generation model中也是一个困难点）MLE是为了最小化真实数据和生成数据的cross-entropy，但是更有效的评估方法是让人工观察（这个会不会有些主观）。

# Real-world Scenario

在generated texts和human-created texts进行评估时，用BLUE scores。

# Appendix

正好看这篇paper的weekday，Paper Weekly请来了作者进行介绍。附上[slide](http://lantaoyu.com/files/2017-07-26-gan-for-discrete-data.pdf)