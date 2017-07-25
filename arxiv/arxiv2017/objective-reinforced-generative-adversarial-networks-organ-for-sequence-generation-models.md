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

# Model

通过policy gradient来训练generator，从而同时最大化两个reward：一个是用来最大化hard-coded objective，另外一个是为了骗discriminator。

通过Figure 1，可以发现，前面通过G生成数据，来欺骗D的步骤没有变化，改变的是G的训练部分，多增加了一个RL过程。

# Experiment

## molecule

在molecule generation中，使用如下进行model evaluation：

1. novelty：如果是novel，并且valid，返回1；novel但是invalid，返回0.3；否则不是novel，返回0
2. diversity：训练集中随机选择一些molecule，并且计算average similarity
3. solubility（$$Log(P)$$）：能够溶解于水的概率，使用RDKit内置函数
4. synthetizability：RDKit内置的，正则化后测量合成性的函数。基于分子复杂性和组成复杂分子的难易程度
5. druglikeness：heuristic的方法，来测量一个分子可能被用于人体的测度，是前面四个测度的线性合成

## music

在音乐合成的部分，提出了三个evaluation

1. tonality：有多少个生成的fifths（太专业了，谁能帮忙？）
2. melodicity：为了降低的和音，有几种interval。如果一个interval要上melodic，必需是前三个中的一种。
3. ratio of steps

# Appendix

附refer的文章，也是用的RL进行molecule generation

Jarques, etc. Sequence Tutor: Conservative Fine-Tuning of Sequence Generation Models with KL-control

L. Yu, etc. SeqGAN: Sequence Generative Adversarial Nets with Policy Gradient