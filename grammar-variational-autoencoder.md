# Grammar Variational Autoencoder

Matt J. Kusner

Brooks Paige

Jose Miguel Hernandez-Lobato

# Intro

生成机器学习模型,能够很成功的将数据表示为连续空间.

数据首先通常是字符串表示,这是因为类似LSTM,GRUs,CDNNs模型可以非常好地利用字符串序列.比如SMILES.

但是这种表达方式比较脆弱:字符串的稍微改动就会使得分子变化剧烈.而且根据概率decoder生成的SMILES,很可能是无效的.

为了解决这个问题,我们提出了一种 使用grammar直接将离散数据的结构 联合考虑进来.

给定一个grammar,每一个有效的离散对象都能够描述出从grammar出来的解析树.因此我们提出了grammar variational autoencoder(GVAE),直接从解析树encode和decode.

# Background

## Variational autoencoder

$$p(z)$$表示是编码,是在probabilistic generative model中的隐藏变量.

$$p_\theta(x|z)$$表示的是似然函数,是probabilistic decoder.

如果将$$p(z)$$看作是先验,那么可以有$$p_\theta (z|x) \propto p(z)$$,表示的是probabilistic encoder.

如果要同时学习MLE和MAP的参数,可以使用evidence lower bound(ELBO)方法.

$$\mathcal{L}(\phi, \theta; x) = \mathbb{E} _{q(z|x)} [log \, p_\theta(x,z) - log \,  q_\phi (z|x)] $$

## Context-free grammars

context-free grammar(CFG)定义为包含四个元素的tuple $$G=(V,\sum,R,S)$$.分别表示,未终止信号集,终止信号集,产生规则,以及不包含开始信号的未终止信号集.R的产生规则表示是从$$V$$到$$V \bigcup \sum$$的映射.

这种映射可以构成一棵解析树. $$\alpha \to \beta$$,一直到所有的叶子节点都是来自于$$\sum$$,即终止符号.

这个context-free grammar可以构成一个probabilistic generative model的骨干,来产生有效的字符串.

# Methods

## Encoding

首先通过使用一个SMILES grammar将字符串解析成解析树.然后将解析树通过前序遍历产生production rules R.使用1-hot indicator vector来表示这些规则.

## Decoding

这里是如何将连续的向量映射回

# Appendix

很有意思的encoding方法.以往都是使用fingerprints或者纯粹SMILES作为特征.这里是使用了基于语法解析树生成的特征.