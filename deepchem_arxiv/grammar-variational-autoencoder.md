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

这里是如何将连续的向量映射回sequence of production rules.

RNN会产生一个unnormalized log probability(也叫logits) vectors 集合,logits的每一个维度都代表了语法中的一个production rule.

使用堆栈,首先从开始信号解析,也就是SMILES.初始状态就是解析信号在堆栈中,然后pop out.我们从堆栈中pop out的信号,使用mask vector来去掉无效的向量.mask vector $$m_a \in [0,1]^K$$ 指的是$$a \to b$$ 中,将a所以可能指向的b使用1表示,其余都为0的binary vector.

然后在剩下的unmasked rules中进行抽样,使用logit vector的数值.为了在任意一个时间t,进行抽样,使用以下masked distribution:

$$p(x_t = k| \alpha, z) = \frac{ m_{\alpha, k} exp(f_{tk}) }{ \sum_{j=1}^K  m_{\alpha, k} exp(f_{tj})}$$

其中$$f_{tj}$$表示的是矩阵的第t行,第j列,也就是第j中production rule.直观的说,就是对第t行向量,进行exp normalization.

说得这么复杂,简单的,就是我们在对某一行向量进行normalization的时候,不把所有的production rule都考虑进去,而是只考虑可能的,有效的rule.做到这点,使用上述的mask vector进行element-wise的乘法即可.

所以这里所用的堆栈可以理解为我们使用一个深度优先搜索搜索所有可能的情况.

## Training

每次训练都是先encode,然后通过模型,decode 计算得到logits.使用ELBO进行gradient update.(这一步感觉描述的不是特别详细,另外提到了一批paper)

# Appendix

很有意思的encoding方法.以往都是使用fingerprints或者纯粹SMILES作为特征.这里是使用了基于语法解析树生成的特征.

GVA与character VAE不同的是,CVAE在下一位会产生任意的字符,而GVA会遵从语义.

计算ELBO gradient的方法,参考这篇['14 ICLR 论文](https://arxiv.org/pdf/1312.6114.pdf).