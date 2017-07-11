# EMNLP versus ACL: Analyzing NLP Research Over Time

Sujatha Das Gollapalli, A*STAR

XiaoLiLi, A*STAR

# Introduction

展示最近二十年，ACL和EMNLP的热门话题。

# Methods

在$$Y=\{y_1,...,y_T\}$$这些年中，两个研究会议$$V=\{ACL,EMNLP\}$$。$$D$$是所有的文档集合，每一篇文档$$d \in D$$都关联到$$\{K_d,y,v\}$$，其中$$\{K_d\}$$指向$$d$$的内容，$$v$$和$$y$$分别是会场场地和发表年代。

## Venues as Probability Distributions

用概率主题模型，假设有$$t_1,...,t_k$$个主题，得到$$P(t_i|d)$$。而场地主题概率分布$$P(t_i|v_y)$$对于一个给定的会场和年代$$(v=l,y=m)$$，可以通过$$D_{l,m}$$计算。也就是
$$
P_{l,m}(t_i) = \frac{1}{|D_{l,m}|} \sum_{d \in D_{l,m}} P(t_i|d)
$$

# Venues as TP-ICP Vectors

离散的数据通常会归一化为向量，比如这里我们就用$$P(t_i|v)$$组成一个k维的向量。这个topic porportion（TP）向量类似在Information Retrival中归一化的术语频率向量。

同时也延伸了inverse document frequency，来描述Inverse Corpus Proportion（ICP）。ICP的目标是为了获取重要的话题，通过减少在其他所有年度通常出现的话题的影响。让$$TP_{v,y}(i)$$表示话题$$t_i$$在会场$$v$$，在$$y$$年度，所占的重要性比例。那么可以得到

$$ICP(t_i)=log(\frac{\sum_{y \in Y}\sum_{v \in V} \sum_{j\in [1,k]} TP_{v,y}(j)}{\sum_{y \in Y}\sum_{v \in V} TP_{v,y}(i)}) = \frac{|D|}{\sum_{y \in Y}\sum_{v \in V} TP_{v,y}(i)}$$

因为$$\sum_{j\in [1,k]} TP_{v,y}(j）=1$$，且$$|Y| \times |V| = |D|$$。

所以某场会的TP-ICP想来就定义为$$[TP(1)\times ICP(1), ..., TP(k)\times ICP(k)]$$。比较两个会的相似度就使用余弦相似度：$$consine(P,Q) = \frac{\sum_{i \in [1,k]} p_i \cdot q_i}{\|P\|_2 \cdot \|Q\|_2}$$