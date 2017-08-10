# Online Multitask Relative Similarity Learning

Shuji Hao, etc.

# Intro

relative similarity learning (RSL)的目标是为了在一定的限制条件下，从训练数据中学习到similarity function。现有的RSL有这么几个问题

1. 需要大量的re-training，因为使用batch-based method
2. 前面提出的online learning主要是为了single-task。所以对于multi-task的设定下，要么对每一个task独立学习一个local similarity，要么对于所有的task学习一个global similarity。

但很少有工作眼睛multi-task relative similarity learning。