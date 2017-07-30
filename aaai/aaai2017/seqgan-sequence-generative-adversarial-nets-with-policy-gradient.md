# SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient

Lantao Yu, Weinan Zhang, Jun Wang, Yong Yu

# Intro

RNN经常用于sequence generation，而且通常是最大化log predictive likelihood。但是后来Bengio在2015年的paper中提出了MLE会带来exposure bias问题：模型迭代的生成序列，产生的下一个token可能从来没有在词库中出现过。为了解决这个问题，Bengio提出了使用scheduled sampling，生成模型在产生写下一个token的时候，输入的是合成的数据（观测到的token）作为prefix。但后来Huszar的paper证明了这个方法并没有从根本上解决问题。另外一个可能的解决方案是在整个生成的序列上面构造loss function，而不是每一次转换。

GAN模型也可以用来处理上面问题。