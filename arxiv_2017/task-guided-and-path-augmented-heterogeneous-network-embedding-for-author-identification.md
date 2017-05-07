# Task-Guided and Path-Augmented Heterogeneous Network Embedding for Author Identification

Ting Chen, UCLA

Yizhou Sun, UCLA

# Intro

heterogeneous network包含不同类型的node和link.比如,在bibliographic network(书目网络)中,节点类型有论文,作者,连边类型有 作者写论文, 论文包含关键字等.

这篇paper主要是挖掘hetegenerous bibliographic network的信息,更确切的说,是在双盲review的情况下,确定作者身份.

# Preliminaries

## Hetegenerous Networks

+ Hetegerenous Networks: 如前面所说,就是网络有不同的节点和连接.节点有论文,作者,关键字,地点,和年份.链接有 作者写论文,论文包含关键字.
+ Meta path: meta path定义为$$T_G = (\mathcal{O}, \mathcal{L})$$,并被标记为$$O_1 \to O_2 \to ... o_{m+1}$$.

## Embedding Representation of Nodes

network data通常是高纬度并且稀疏的.这使得如何表示网络中的节点会有困难,解决方案就是用embedding表示.

## Author Identification Problem

将author identification变成bibliographic network问题.对于每一篇论文p,这篇论文的邻居表示为$$X_p = \{ X_p^{(1)}, X_p^{(2)}, ... , X_p^{(T)} \}$$,其中$$X_p^{t}$$表示第t个类型的邻居节点.节点的类型包括关键字,参考文献,地点,和年份.使用$$A_p$$来表示论文p的作者集合.

**author Identification Problem:** 给定一个论文集合，表示为$$(X,A)$$，其中$$X=\{X_p\}$$是，$$A=\{A_p\}$$就是某一个论文的作者集合。目标就是对每一篇paper，根据其邻居信息，学习一个作者的排序。

# Proposed Model

模型主要由两部分组成，基于任务相关embedding的author identification，和path-augmented general network embedding。

## Task-Specific Embedding for Author Identification

提出一个基于embedding的监督式方法，来对作者进行排序。首先将每一个节点映射到隐藏特征空间上，然后对未署名的论文，基于邻居的信息构造特征表示。最终将每篇论文对应的特征聚合起来，对可能的作者进行排序。

将论文对应的特征聚合分为两个步骤
1. 对每一个节点类型，构造特征向量，通过将属于这类的节点类型，average node embedding，这就好比提供了一个prior knowledge。公式为 $$ V_P^{(t)} = \underset{n \in X_p^{t}}{\sum} u_n / |X_p^{t}| $$
2. 对论文p构造特征向量，通过对不同的节点类型进行加权组合。公式为 $$V_p = \sum_t w_t V_p^{(t)}$$。

所以现在论文p可以通过$$V_p$$来表示，并且能用来对可能的作者进行评分。作者a和论文p之间的分数为：

$$f(p,a) = u_a^T V_p = u_a^T (\sum_t w_t V_p^{(t)} ) = u_a^T (\sum_t w_t \underset{n \in X_p^{t}}{\sum} u_n / |X_p^{t}| )$$。

在图三种把第一部分的取平均称作是mean pooling。

使用hindge loss来求模型的参数$$U$$和$$w$$。

## Path-Augmented General Heterogeneous Network Embedding

目前的embedding计算都是基于一种类似end-to-end的想法，节点的embedding可以通过neighbor prediction来确定。

在heterogeneous network中，可以通过不同的meta path来丰富neighbors的语义。比如两个作者之间的联系可能有不同的相似度：（1）对同一个话题感兴趣 （2）有同样的喜好。这些联系意味着不同的语义。

因此neighbor prediction也要做出相应的改变。不仅仅condition on节点，还要condition on path r。

$$P(j|i, r ) = \frac{exp(u_i^T u_j)}{ \sum_{j' \in DST(r) } exp(u_i^T u_{j'}) }$$

期中$$DST(r)$$表示的是path r的终点。因为$$DST(r)$$可能会非常大，所以应用了negative sampling，并有了以下approximation：

$$log \hat P(j|i,r) \approx log \sigma(u_i^T u_j + b_r) + \sum \mathbb{E} [log \sigma(-u_i^T u_{j'} - b_r)] $$

## The Combined Model

以上两种embedding方法，分别揭示了network不同的特征。前一个更多的关注是任务相关的直接信息，后一个更好地挖掘的全局多样的特征。因此就像把这两个模型结合起来。

+ 联合目标当作显性的guidance。
+ meta path当作一种隐性的guidance。