# Document-level Snetiment Inference with Social, Faction, and Discourse Context

Eunsol Choi, Hannah Rashkin, Luke Zettlemoyer, Yejin Choi

UW-Seattle

## Introduction

文档级别的感情推导。

## Document-level Sentiment Model

给定一个文档$$d$$，和一系列已知的实体$$e_1,...,e_n$$。

使用了两个基本的模型：一个是pairwise sentiment classifier，一个是pattern-based faction extractor。ILP解决的目标是最大化：

$$F=\psi_{social} + \psi_{fact} + \sum_i \sum_j \psi_{ij}$$。

其中$$\psi_{ij}$$是pairwise potentials。$$\psi_{ij} = \phi_{pos_{ij}} \cdot pos_{ij} + \phi_{neg_{ij}} \cdot neg_{ij} + \phi_{neu_{ij}} \cdot neu_{ij}$$。

包含了情感分类器的结果$$(\phi_{pos}, \phi_{neg}, \phi_{neu})$$，和binary变量$$pos_{ij}, neu_{ij}, neg_{ij}$$。比如，$$neg_{ij}=1$$就表示i对j有负面态度。

### Inference with factions

soft ILP限制$$\psi_{fact}$$是模拟一般互相支持的实体会对其他实体有类似的情感。公式定义：

$$\psi_{fact} = \sum_i \sum_j ( \alpha_{iteself} \cdot itself_{ij} + \sum_k (\alpha_{fact} \cdot ( tie\_same_{ijk} - tie\_diff_{ijk}) ) )$$

这个公式可以预测不明显的情感分析，通过联合考虑了事实和情感关系。

### Inference with sentiment relations

根据Social balance theory，动态的情感分析是一个人际之间的网络。尤其是在平衡的状态下，在一个positive term中的实体会对其他实体有类似的观点，而在negative term中的实体就会有相反的观点。引入一系列变量来获取这类关系：

假设$$e_i$$对$$e_j$$是pos的：
+ $$pos\_same_{ijk} = pos_{ij} \bigwedge pos_{ik} \bigwedge pos_{jk} + neg_{ij} \bigwedge neg_{ik} \bigwedge neg_{jk} $$
+ $$pos\_diff_{ijk} = pos_{ij} \bigwedge neg_{ik} \bigwedge pos_{jk} + neg_{ij} \bigwedge pos_{ik} \bigwedge neg_{jk} $$

那么就有:

$$ \psi_{bl} = \sum_i \sum_j \sum_k ( \alpha_{bl} \cdot (pos\_same_{ijk} + neg\_diff_{ijk} ) + \alpha_{bad_{bl}} \cdot (pos\_diff_{ijk} + neg\_same_{ijk}) ) $$

最后将$$\psi_{bl}$$加到$$\psi_{social}$$上。

再考虑一个相互作用的限制：

$$
r\_same_{ij} = pos_{ij} \bigwedge pos_{ji} + neg_{ij} \bigwedge neg_{ji}\\
r\_diff_{ij} = pos_{ij} \bigwedge neg_{ji} + neg_{ij} \bigwedge pos_{ji}\\
\psi_r = \sum_i \sum_j \alpha_r(r\_same_{ij}) + \alpha_{bad_r}(r\_diff_{ij})
$$

再把$$\psi_{r}$$加到$$\psi_{social}$$上。

## Appendix

有一个很有意思的[展示](http://homes.cs.washington.edu/~eunsol/project_page/acl16/)。