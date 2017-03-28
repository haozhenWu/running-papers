# Summarizing Answers in Non-Factoid Community Question-Answering

Hongya Song, SDU

Zhaochun Ren, UCL

Shangsong Liang, UCL

Piji Li, CUHK

Jun Ma, SDU

Maarten de Rijke, UVA

## Intro

CQA(community question-answering).factoid CQA是存在只有一个现有的答案,而non-factoid CQA中,几个sparse sentence一起组成一个答案.

## Overview of the method

论文描述非常清楚,主要提出了candidate sentence set S,salient score vector A.而CQA的目标就是在限制条件最多选择L个向量的情况下,最大化saliency scores.

那么问题主要就在如何的得到完整的文档,如何将sentence向量化,以及如何得到针对于每一个句子的salient score.

### Document Expansion

#### Entity linking

给定一个candidate answer d,entity linking的目标是在一个knowledge base中确定entity e,最可能是d中每一个句子的参考.这篇论文吧Wikipedia作为knowledge base,使用n-gram token.从而对每一个d,都找到了candidate links集合.第二步就是同时考虑d和q中的单词集合 $$Q_s = \{ V_s, V_q\}$$ 作为query,使用BM25对link candidates进行排行.

#### QA-based sentence ranking

有了link candiates,选择三个最高的中心句.首先构造相似度矩阵,每一个点表示的是两个句子之间的相似度,在$$Q_s = \{ V_s, V_q\}$$作为输入的查询时.

$$M_{ij} = sim(s_i, s_j | Q_s) / \sum_{s_x \in S_s} sim( s_i, s_x | Q_s) $$

每一个句子的初始score设置为$$1/|S_s|$$,在第l步迭代的时候,通过以下对score进行更新

$$sco(S_i)^{(l)} = (1-\lambda_e) \sum M_{i,j} \cdot sco(s_j)^{(l-1)} + \lambda_e \frac{1}{|S_s|}$$

其中$$|S_s|$$是Wikipedia document中连接到anchor text g的句子的数量.damping factor $$\lambda_e$$的数值是0.15.

那么transition matrix就可以写成$$\tilde M = (1-\lambda_e) M + \bar e \bar e^T \lambda_e / |S_s|$$,其中$$\bar e$$是每一个元素都为1.sco可以通过M的principle eigenvector获取.然后从ranked list中提取top sentences,组成了auxiliary sentences S'(辅助句子).

### Sentence representation

将candidate sentence S和auxiliary sentences S'.使用CNN来挖掘语法和语义的隐藏关系.

#### CNN



# Appendix

