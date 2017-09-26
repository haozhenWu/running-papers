# Learning to select data for transfer learning with Bayesian Optimization

Sebastian Ruder, Barbara Plank

# Intro

这里第一个提出了model-agnostic data selection来进行transfer learning。这篇paper会使用不同的similarity进行评估，测试学习到的模型的鲁棒性。

# Data selection model

domain similarity measure $$S$$，$$X$$是训练样本，$$D$$是source domains。以往的方法是依据similarity score选择最优的sample，实际跑的结果很好，但是无法准确了解task和domain的关系。这里提出了学习一下的linear domain similarity measure：

$$S = \phi(X) \cdot w^T$$

其中$$\phi(X)$$是similarity和diversity features。$$w$$通过bayesian optimization学习。

## Bayesian Optimization for data selection

因为measure $$S$$应该不知道objective function的信息，所以我们不使用gradient-based method。

对于一个black-box function $$f:X \to R$$，Bayesian Optimization能够找到一个输入$$x$$，使得$$f$$最小。同时需要prior $$p(f)$$和acquisition function $$a_{p(f)}: X \to R$$。

算法流程：
1. 给定一组输入，是weight $$w$$
2. 所有source domain的training examples都被评估和排序，排序是按照上面的公式(1)进行的
3. 每一个task的模型在top n sample上进行训练
4. 模型在validation set上进行评估

## Features

使用一系列similarity metric作为特征。

定义$$P$$是source training的representation，$$Q$$是target domain的representation。$$M = \frac{1}{2} (P+Q)$$是average distribution，$$D_{KL}(P\|Q)$$是KL divergence。由此使用了这么几个特征：Jensen-Shannon divergence，Renyi divergence，Bhattacharyya distance，cosine similarity，euclidean distance，variational dist。

然后考虑三种不同的representation：term distribution，topic distribution，word embedding。这里的representation指的是从feature encode到latent space的过程。

对于每一个training data，计算几种diversity。

# Experiments

实验在sentiment analysis，POS tagging，parsing，并且证明比baseline有很大的提升。