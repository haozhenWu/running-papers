# Less Than a Single Pass: Stochastically Controlled Stochastic Gradient

Lihua Lei, Michael I. Jordan

UCB

# Intro

介绍了stochastic variance reduced gradient(SVRG)的变形方法，叫做stochastically controlled stochastic gradient(SCSG)。SCSG在computation和communication上的复杂度都是sublinear。

传统的SGD方法，需要对step size进行良好的tuning。而SCSG可以有一个简单的automatic tuning procedure，保证找到$$\epsilon-approximate$$ 答案。

## Assumption and Algorithm

y is $$\epsilon$$-approximate solution if

$$ \mathbb{E} f(y) - f(x^*) \le \epsilon $$

而后关于L-Lipschitz和strongly convex function提出了三个假设。

## SCSG

SCSG是一个类似SVRG的方法，实现了在一个B中的sub-sample $$\mathcal{I}$$ gradient computation。

$$\mathcal{I}$$的采样符合以下条件：

+ 对于non-strongly convex，$$N_j$$符合geometric distribution
+ 对于convex function，$$N_j$$符合truncated geometric distribution
+ data point index从$$\mathcal{I}$$产生

