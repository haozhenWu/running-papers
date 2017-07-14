# Large-scale Multi-label Learning with Missing Labels

Hsiang-Fu Yu, Prateek Jain, Purushottam Kar, Inderjit S. Dhillon

UTA, MS

# Intro

multi-label分类问题有两个challenge

1. 如果有上百万的label，数据范围太大
2. 有missing data

这篇paper同时解决了这两个问题，使用了Empirical Risk Minimization （ERM）框架。

针对于问题1，常用的解决方案是label space reduction，也就是减少label-space的维度，通过random projection或者canonical correlation analysis(CCA) based projection。但这种投影的方法有局限。

这篇paper提出了一个更加直接的方法，将问题狗造成一个low-rank linear model的学习过程。$$y^{pred} = Z^T x$$，然后转换成传统的ERM问题，从而能够使用各类的loss function和regularization。

# Problem Formulation

对于每一个data point $$i$$，$$d_i^j = 0/1$$分别表示absent或者present。

目标函数 loss function就是 $$ l(y, f(x;Z)) = \sum_j l( y_j, f_j(x;Z))$$。假设label之间有很强的相互关系，那么我们可以学习一个low-rank matrix $$Z$$。所以能有以下的模型：

$$
\hat Z = arg\, \underset{Z}{min} J(Z) = \sum_i^n \sum_j^L l(Y_{ij}, f^j(x_i;Z)) + \lambda \cdot r(Z)\\
s.t. rank(Z) \le k
$$

其中n个data points，L个label。

类似的设计也可以运用到missing labels的情况。

# Algorithm

有一种更加高效的解决方案。

$$Z = W H^T$$, where $$W \in \mathbb{R}^{d \times k}, H \in \mathbb{R}^{L \times k}$$。$$\|Z\|_{tr} = \frac{1}{2} ( \|W\|_F^2 + \|H\|_F^2 )$$。

所以最后要优化的函数就是

$$J_\Omega(W,H) = \underset{ (i,j) \in \Omega}{\sum} l(Y_{(i,j)}, x_i^T W h_j) + \frac{1}{2} (\|W\|_F^2 + \|H\|_F^2) $$

然后可以发现当$$W$$和$$H$$任何一个fix的时候，$$J(W,H)$$都是convex function。所以可以通过迭代求解：

$$
H^{t} \leftarrow arg \, \underset{W}{min} J_\Omega(W^{(t-1)}, H)
\\
W^{t} \leftarrow arg \, \underset{H}{min} J_\Omega(W, H^{(t)})
$$

# Appendix

