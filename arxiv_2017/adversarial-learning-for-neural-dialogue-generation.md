# Adversarial Leaning for Neural Dialogue Generation

Jiwei Li, Will Monroe, Tianlin Shi, Alan Ritter, Dan Jurafsky

## Introduction

从图灵测试受到的启发。将任务转化为两个模型系统组成的增强式学习，一个生成模型用于生成序列，一个判别模型用于模拟人类，来判别是否通过图灵测试（区分是人类还是机器）。

## Related Work

对抗网络的训练中，生成模型就是用来输出“欺骗”判别模型。这个技术在图像上使用十分成功，但是NLP并未。原因和图像中不同，NLP的数据是离散的,使得判别模型输出的error很难backpropagate到生成模型上。

## Adversarial Training for Dialogue Generation

给定一个对话历史$$x$$，模型需要生成一个对应的$$y=\{y_1, y_2, ..., y_T\}$$。对抗增强算法由两部分组成：

+ 生成模型：类似seq2seq模型，输入和输出一样，就为了进行encoding和decoding。

+ 判别模型：一个二分类模型，输入是$$\{x,y\}$$，输出是这个模型是否为人或者机器产生。输入通过hierarchical encoder压缩到了一个向量，然后喂给一个2-class softmax函数，返回人为产生和机器产生各自的概率。

### Policy Gradient Training

这个系统最主要的观点是让生成器能够生成的表达输出让判别模型无法判别式是否为人为生成的。使用policy gradient method来实现这个目标；也就是说，将当前对话实例中人为生成的表达，用来当作给生成器的‘reward’，而增强算法就用来最大化 reward of generated utterances。

