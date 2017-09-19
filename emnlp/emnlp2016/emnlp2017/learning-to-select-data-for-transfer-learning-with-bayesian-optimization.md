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