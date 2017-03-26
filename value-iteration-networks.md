# Value Iteration Networks

Aviv Tamar, UC-Berkeley

Yi Wu, UC-Berkeley

Garrett Thomas, UC-Berkeley

Sergey Levine, UW

Pieter Abbeel, UC-Berkeley

# Introduction

Value Iteration Networks(VIN)是一个包含'planning module'的可导网络。

deep CNN对监督式学习任务产生了革命性影响，比如物体识别、动作识别和图片语义分割。最近CNN也被应用到增强式学习任务，比如Atari游戏、机器操控和模仿学习。

在RL问题中，制定决策的sequential nature是和监督式学习最大的不同，会要求某种形式的计划。如果只用CNN进行特征提取，然后接上完全连接的层，将特征对应到行为的概率分布，这种模型就缺少决策计算。

传统使用CNN的RL，能够学习到reactive policy，比如在一张地图上从起点走到终点，但是无法extend到新的任务，因为这个方法无法理解行为的本质。也就是学习到的reactive policy还不是planning。

这里提出的方法就是经典的value-iteration planning算法能够通过特殊形式的CNN代表。通过将VI网络嵌入到标准的向前推进网络中。

## Background

MDP模型是标准的时序的决策制定和计划模型，Value Iteration算法是用来解决MDP模型。

但当状态空间非常大或者连续的时候，MDP模型就没法应用。这个时候，就可以通过专家监督（Imitation Leraning模仿学习）或者尝试和错误（增强式学习）。

## The Value Iteration Network Model

首先能够发现每一个VI过程的迭代，都可以认为是将上一次迭代的值函数$$V_n$$和reward函数$$R$$通过卷积层和max-pooling层传递下来的。核心转换的过程是这样（VI module）：卷积层的每一个频道都对应于一个action的Q-function，而convolution kernel就根据队员的discounted transition probabilities进行权重权衡。因此如果将卷积层执行$$K$$次，那么就相当于VI执行了$$K$$次。

## Appendix

2016 NIPS, Best Paper

还有Karpathy的[笔记](https://github.com/karpathy/paper-notes/blob/master/vin.md)。

刚开始接触增强式学习，很有意思的实现。还有细节不是太明白，比如$$M$$在这里是否有具体的意义？因为我们已经用$$\bar M$$来进行学习。anyway，准备先从grid-world开始玩起来。