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

训练的时候使用Skip-Gram.

