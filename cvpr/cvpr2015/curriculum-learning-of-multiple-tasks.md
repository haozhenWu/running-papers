# Curriculum Learning of Multiple Tasks

Anastasia Pentina, Viktoriia Sharmanska, Christoplh H. Lampert

# Intro

假设task之间的相似性由task parameter之间的euclidean distance决定。这里假设是使用的linear classifier。

以往的模型是假设所有的task都一样重要，然后偶尔有outlier，因此想要研究潜在的task relationship，从而避免negative transfer。

将euclidean distance进行regularization也是另外一种常用的方法，learner有很多task，但是只关注其中一个，剩余的都当作额外信息。

我们的方法就是将multi-task learning降解成了许多domain adaptation问题。由教育领域启发，学习是一个顺序的过程。通过将任务进行排序，逐渐学习更多的知识，并且重复利用前面的知识。

通过使用PAC-Bayesian theory证明了generalization bound依靠于data representation，并且这篇论文的算法能够解决它。

# Related Work

这篇论文的想法是给所有的task设定一个权重向量。

一种常见的方法是将latent task通过linear combination结合起来。

其余几个关于使用权重向量的做法，都需要domain knowledge。

以前关于sequence learning的想法，更多的是在single-task的情况下，如何将sample进行排序。这里就是multi-task的情况下，如何将task进行排序。Kumar在10年的NIPS中，提出的self-paced learning也是如何对sample进行排序，不过后来在17年的AAAI上就已经延伸到了multi-task设定。

# Method

假设每一个task的训练还是linear的情况，$$f(x) = sign \langle w, x \rangle $$，而后使用0-1 loss。目标是为了找到一个对应于不同task的权重向量，目标是为了最终能在所有的task上有最小的average expected error。

$$\ell (w_1, ..., w_n) = \frac{1}{n} \sum_i \mathbb{E} [ y \ne sign \langle w_i, x \rangle ] $$

n是task的个数。

## Learning in a fixed order

假设在顺序$$\pi$$下，前一个task的参数是$$\hat w$$，当前的task就是

$$ \underset{w}{min} \| w - \hat w \|^2 + \frac{C}{m} \sum_j \xi_j $$, where $$y_j \langle w, x_j \rangle \ge 1 - \xi_j, \xi_j \ge 0$$

对于第一个task，假设$$\hat w = 0$$

注意这个方法并没有依靠所有task都是等价的假设。但是他的性能要求task得是相关的，从而进行sequential训练。

## Learning a data-dependent order

使用算法$$A(w_{\pi(i-1)}, S_{\pi(i)})$$，根据前面已经解决的task参数$$w_{\pi(i-1)}$$和训练数据$$S_{\pi(i)}$$，返回$$w_{\pi(i)}$$。

# Appendix

