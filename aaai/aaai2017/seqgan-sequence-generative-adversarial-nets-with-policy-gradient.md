# SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient

Lantao Yu, Weinan Zhang, Jun Wang, Yong Yu

# Intro

RNN经常用于sequence generation，而且通常是最大化log predictive likelihood。但是后来Bengio在2015年的paper中提出了MLE会带来exposure bias问题：模型迭代的生成序列，产生的下一个token可能从来没有在词库中出现过。为了解决这个问题，Bengio提出了使用scheduled sampling，生成模型在产生写下一个token的时候，输入的是合成的数据（观测到的token）作为prefix。但后来Huszar的paper证明了这个方法并没有从根本上解决问题。另外一个可能的解决方案是在整个生成的序列上面构造loss function，而不是每一次转换。

GAN模型也可以用来处理上面问题。但是将GAN用于seq generation有两个问题
1. GAN是为了产生连续的数据，对于discrete还有问题
2. GAN只有为整个序列产生score/loss，对于partially generated seq没有

通过这个办法解决：把sequence generation当作sequential decision making过程，生成模型就被当作RL中的agent，state是已经生成的token，action是下一个选什么token赖声川。为了给reward，我们使用discriminator来评估生成的序列，而用评估的反馈来指导generative model。为了解决输出是discrete时候，无法使得gradient传递给生成器，使用stochastic parameterized policy，因为policy空间是可导的（但其实这里有一个问题，在David 2014年的一篇paper中，已经证明其实deterministic off-policy也可以是可导的，或许将来可以尝试）。

# Sequence Generative Adversarial Nets

