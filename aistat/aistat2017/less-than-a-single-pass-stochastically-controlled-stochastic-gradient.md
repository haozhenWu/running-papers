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

