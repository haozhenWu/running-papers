# An Impossibility Theorem for Clustering

Jon Kleinberg

Cornell

## Intro

不同的clustering技巧，agglomerative, spectral, information-theoretic,和centroid-based,还有其他从组合优化和概率生成模型。这些聚类方法会产生不同的结果。

但是很少有独立于 特别的算法、目标函数、或者生成数据 地研究聚类。而要这么研究的目的可以通过一个类比理解。技术细节上不一样，但是方法理论上类似的，在经济领域，有一个axiomatic framework（自证/公理的框架）：先列出一些应当满足的属性，然后在某些情况下，这些属性无法同时满足。

## An Impossibility Theorem

clustering function的定义和metric space定义类似，只不过需要的是一个可数有限set，而不是metric space中的和$$\mathbb{N}$$ homogeneous 的set。

clustering function要有以下三个性质：

1. scale-invariance: 对于任意的距离函数f和$$\alpha > 0$$，都有$$f(d) = f(\alpha \cdot d)$$。其中$$d$$是任意两点之间的距离。这就是说，不论我如何转换我的distance 单位距离，不影响聚类结果。
2. richness: range(f)等价于集合s的所有分类。就是说，任意的partition都有可能的产生。（可以理解为不是order-sensitive）
3. consistency: $$d$$和$$d'$$是两种距离函数。如果$$f(d)=\tau$$并且$$d'$$是$$d$$的$$\tau-transformation$$，那么$$f(d')=\tau$$。
其中$$\tau$$是集合的一个partition。$$\tau-transformation$$就是，如果我们将原来的距离函数$$d$$更替为新的距离函数$$d'$$，使得同一个cluster中的两点距离缩小，并且将不同cluster之间的两点距离扩大。consistency property就是说，两个距离函数应该能得到一样的结果。

先从single-linkage证明，证明可以幽默型同时满足两条性质。通过选择不同的stopping condition，得到不同的clustering function；也就是可以通过选择stopping condition，能够使得clustering function同时满足任意两条性质。

这里考虑三中stopping condition：

1. k-cluster stopping condition: 当有k个component的时候，停止
2. distance-r stopping condition: 只增加权值最多为r的边。
3. scale-$$\alpha$$ stopping condition: 如果$$\rho^*$$是最大的pairwise distance，那么只增加权值最多为$$\alpha \rho^*$$的边。

可以有：

1. k-cluster stopping condition 满足scale-invariance和consistency。
2. distance-r stopping condition 满足scale-invariance和richness
3. scale-$$\alpha$$ stopping condition 满足richness和consistency

## Antichains of Partitions

## Appendix
