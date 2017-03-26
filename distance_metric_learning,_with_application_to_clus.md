# Distance metric learning, with application to clustering with side-information

Eric Xing, Andrew Ng, Michael Jordan, Stuart Russell

UCB

# 1 Introduction

解决这样的问题：用户在给定的数据空间上，已经给出了一些点数类似的，那么如何自动的根据这些点学习到一个distance metric。

# 2 Learning Distance Metrics

已经有了一些点$$\{x_i\}_{i=1}^m \subseteq R^n$$，并且已经知道有些对手相似的：

$$
S: \, (x_i, x_j) \in S, \text{   } x_i \text{ and } x_j \text{ are similar}
$$
考虑这种形式的距离公式：

$$
d(x,y) = d_A(x,y) = \| x-y \|_A = \sqrt{(x-y)^T A (x-y)}
$$
这里要求$$A$$是pos semi-def。A代表的是一个Mahalanobis distances family。目标函数是

$$
\underset{A}{min} \sum_{(x_i,x_j) \in S} \| x_i - x_j \|^2_A
$$
# 3 Experiments and Examples

试验distance metric的结果：人工生成的数据。分别展示了原始数据，diagonal $$A$$，和full $$A$$的结果。

测试clustering：使用了人工生成的数据和UC Irvine repo的9个数据集。还是diagonal和full metric效果比较好。而且使用这类学习到的metric效果也更好。

# Appendix

真的被几位作者吓到了…


