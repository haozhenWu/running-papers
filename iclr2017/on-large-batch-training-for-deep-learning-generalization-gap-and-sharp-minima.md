# On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima

Nitish Shirish Keskar, Dheevatsa Mudigere, Jorge Nocedal, Mikhail Smelyanskiy, Ping Tak Peter Tang

Northwestern University, Intel Corporation

# Intro

DL经常是解决一个non-convex的优化问题。mini-batch based SGD 可以理解为使用SGD，但加入了噪声。$$B$$通常远小于data point的个数，比如$$\{32, 64, ..., 512\}$$.

# Drawbacks of Large-Batch Methods

许多研究都发现了使用large-batch methods进行训练的时候，generalization gap会增加。但是large-batch和small-batch的方法通常产生相同的结果。有好几种可能的原因：

1. large-batch overfit
2. large-batch 收敛到了saddle point
3. large-batch方法缺少small-batch方法的探究性，并且会跑到和initial point非常接近的minimizer
4. large-和small-batch会converge到不同的点，其中generalization properties不同

这篇文章证明了最后两点。large-batch method会converge到sharp minimizers，而small-batch method会converge到flat minimizer。sharp minimizer的特点是Hessian的eigenvalue更大，所以generalize更差。flat minimum与sharp minimizer相比，lower precision，但better generalization。

比较了几个image recognition经典模型，发现使用small-batch的generalization error小很多。这篇paper还提到了一个很有意思的观点，这个generalization gap不是由于overfitting引起的，而是由于network模型的选择引起的。（不过在这里不是重点）

让$$x^*_s$$和$$x^*_l$$分别表示使用small和big batch-size的答案。对于$$\alpha \in [-1, 2]$$，plot $$f(\alpha x^*_s + (1-\alpha) x^*_l )$$，并且叠加classification accuracy。从图2发现large batch minima sharper。

# Appendix

Sepp Hochreiter, Jurgen Schmidhuber, Flat minima. 1997