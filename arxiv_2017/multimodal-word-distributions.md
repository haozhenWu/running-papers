# Multimodal Word Distributions

Ben Athiwaratkun, Andrew Gordon Wilson

Cornell University

# Intro

提出使用multimodal Gaussian(GMM, Guassian Mixture Model)来表示一个单词,每一个维度就能表示一个意思.

将以往常用的word2vec称作point embedding,而类似本文研究的probabilistic word embedding刚刚开始.

# Method

## Word Representation

每一个单词$$w$$的分布是

$$f_w (x) = \sum_{i=1}^K p_{w,i} \, \mathcal{N} [x; \mu_{w,i}, \Sigma_{w,i} ] = 
\sum_{i=1}^K \frac{p_{w,i}}{\sqrt{2\pi|\Sigma_{w,i}|}} e^{-\frac{1}{2} (x-\mu_{w,i})^T \Sigma_{w,i}^{-1} (x-\mu_{w,i}) } $$

这里假设GMM是由K个component / Gaussian Model 组成,每一个component的mean vector都表示单词的一个意思.

## Skip-Gram

训练的时候使用Skip-Gram. Skip-Gram是根据一个分布假设:每一个单词都和上下文有语义上的联系.

## Energy-based Max-Margin Objective

使用类似negative sampling方法进行训练.$$(w,c)$$分别是word和context,$$(w,c')$$则是negative context word.

训练目标是最大化$$(w,c)$$的energy函数,而最小化$$(w,c')$$的energy函数.

使用max-margin ranking objective:

$$L_\theta(w,c,c') = max(0, m - log E_\theta(w,c) + log E_\theta (w,c'))  $$

## Word Sampling

有一些单词出现频率太高,比如the, a, to,而一些名词更有意义,所以采取一些方法来平衡一下.$$P(w_i) = 1 - \sqrt{t / f(w_i)}$$.

至于negative sample的采样,就从unigram distribution的变形$$U(w_i)^{3/4}$$采样.

## Energy Function

energy function,或者similarity function,在vector embedding情况下,通常使用点乘来体现point similarity.但对于probabilistic embedding,不仅仅要提醒相似性,还要体现不确定性.



# Appendix

follow up paper: Word representation via gaussian embedding
