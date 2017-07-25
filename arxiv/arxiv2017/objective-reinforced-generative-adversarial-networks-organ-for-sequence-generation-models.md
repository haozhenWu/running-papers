# Objective-Reinforced Generative Adversarial Networks (ORGAN) for Sequence Generation Models

Gabriel Guimaraes, Bengamin Sanchez-Lengeling, Pedro Luis Cunha Farias, Alan Aspuru-Guzik

Harvad

# Intro

RL可以生成任意的序列。在naive RL中，使用固定的reward并且将模型用于决策。这会产生unphysical/uninteresting samples。

这篇paper提出了Objective-Reinforced Generative Adversarial Network(ORGAN)，除了RL，还是用GAN来矫正生成（分子）的过程。

ORGAN在RL的reward函数上，还增加了一个GAN discriminator项。generator是最大化两个reward的平均
1. 原始的objective函数，是固定的
2. discriminator部分，有generator动态决定的

动态的discriminator部分能够避免产生uninteresting或者重复的数据。

# Appendix

附refer的文章，也是用的RL进行molecule generation

Jarques, etc. Sequence Tutor: Conservative Fine-Tuning of Sequence Generation Models with KL-control

L. Yu, etc. SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient