# Neural Word Embedding as Implicit Matrix Factorization

Omer Levy, Yoav Goldberg

Bar-Ilan University

# Background: Skip-Gram with Negative Sampling(SGNS)

skip-gram模型假设有一个词库，和一个上下文库，分别用$$w \in V_W, c \in V_C$$表示。在Mikolov的几篇paper中，word来自于文本词库，context来自于大小为L的window，还有其他的context定义。我们将观测到的word和context pair标记为D。使用$$\#(w,c)$$来标记这个$$(w,c)$$对出现在D中的次数。类似的，有$$\#(w)$$和$$\#(c)$$。

考虑$$(w,c)$$对，让$$P(D=1|w,c)$$表示这一对来自于数据。那么有

$$P(D=1|w,c) = \sigma(\vec{w}, \vec{c}) = \frac{1}{1+e^{- \vec{w} \cdot \vec{c}}}$$

negative sampling用了一个比较tricky的方法，为观测到的的$$(w,c)$$最大化$$P(D=1|w,c)$$，同时为随机取样的negative examples最大化$$P(D=0|w,c)$$。因为有一个前提假设是为同样一个字，随机选取上下文会指向未观测到的$$(w,c)$$。因此SGNS的目标函数是

$$log \, \sigma(\vec{w}, \vec{c}) + k \cdot \mathbb{E}_{c_N \sim P_D}(log \, \sigma(- \vec{w} \cdot \vec{c} )) $$

其中$$P_D(c) = \frac{\#(c)}{|D|}$$是empirical unigram distribution。

# SGNS as Implicit Matrix Factorization

##　Characterizing the Implicit Matrix

SGNS同时将word和context embed到低纬度的空间，从而有了两个矩阵$$W$$和$$C$$。而可以尝试考虑$$W \cdot C^T = M$$，SGNS就可以当作是将隐式矩阵$$M$$进行分解。

$$M_{i,j}$$对应于点乘 $$W_i \cdot C_j = \vec{w_i} \cdot \vec{c_j}$$，因此可以理解为分解之后的两个矩阵，一个每一行对应于一个word，一个每一行对应于一个context，而隐式矩阵的每一个cell $$f(w,c)$$对应的是word-context pair直接联系的强度。

只考虑某一个特定(word,context) pair

$$ l(w,c) = \# (w,c) \cdot \sigma(\vec{w} \cdot \vec{c}) + k \cdot \# (w) \cdot \frac{\#{c}}{|D|} log \, \sigma(- \vec{w} \cdot \vec{c})$$

然后进行求导，同时$$x = \vec{w} \cdot \vec{c}$$

$$ \frac{\partial l}{\partial x} = \#(w,c) \cdot \sigma(-x) - k \cdot \#(w) \cdot \frac{\#(c)}{|D|} \cdot \sigma(x) $$

即为 $$ \#(w,c) (1+e^{-x}) - k \cdot \frac{\#(w) \cdot \#(c)}{|D|} \cdot (1+e^x) = 0$$

即为 $$ \#(w,c) (e^x+1) - k \cdot \frac{\#(w) \cdot \#(c)}{|D|} \cdot (e^x+e^{2x}) = 0$$

$$e^{2x} - ( \frac{\#(w,c)}{k \cdot \#(w) \cdot \frac{\#(c)}{|D|}} - 1) e^x - \frac{\#(w,c)}{k \cdot \#(w) \cdot \frac{\#(c)}{|D|}} = 0$$

如果定义为$$y=e^x$$，可以解得$$y=-1$$或者

$$y = \frac{\#(w,c)}{k \cdot \#(w) \cdot \frac{\#(c)}{|D|}} = \frac{\#(w,c) \cdot |D|}{\#(w) \cdot \#(c)} \cdot \frac{1}{k}$$

$$\vec{w} \cdot \vec{c} = log( \frac{\#(w,c) \cdot |D|}{\#(w) \cdot \#(c)} \cdot \frac{1}{k} ) = log( \frac{\#(w,c) \cdot |D|}{\#(w) \cdot \#(c)} ) - log \, k$$

其中 $$log( \frac{\#(w,c) \cdot |D|}{\#(w) \cdot \#(c)})$$ 叫做pointwise mutual information(PMI)，也就是$$(w,c)$$的逐点互信息。

最后，可以将SGNS的$$M$$分解表示为

$$ M_{ij}^{SGNS} = W_i \cdot C_j = \vec{w_i} \cdot \vec{c_j} = PMI(w_i,c_j) - log \, k $$

所以当k=1的时候，SGNS目标是将word-context矩阵进行分解，其中word和context的联系是用PMI来衡量。而如果k>1,那么SGNS就在分解一个shifted PMI matrix，$$M^{PMI_k} = M^{PMI} - log \, k$$。

## Pointwise Mutual Information

PMI是常用的管联性度量值。PMI经常被用作word similarity tasks。

PMI矩阵有一个问题是word-context pair并没有出现，$$PMI(w,c) = log\, 0 = -\infty$$。解决方法是使用Dirichlet prior，每一个的count都增加1。

另外一个解决方案是当$$\#(w,c)=0$$的时候，另$$PMI(w,c)=0$$。这样就有一个sparse matrix。但这会带来另外一个inconsistent的问题：有一些观测到的但是“bad”(uncorrelated)的word-pair对，他们的value是负的，但是未观测到的word-context对数值反而更高，为0。

这个解决的方案是使用positive PMI(PPMI)，所有的negative都是用0代替：

$$PPMI(w,c) = max(PMI(w,c), 0)$$

# Appendix

> An n-gram of size 1 is referred to as a "unigram"; size 2 is a "bigram" (or, less commonly, a "digram"); size 3 is a "trigram". Larger sizes are sometimes referred to by the value of n in modern language, e.g., "four-gram", "five-gram", and so on.(摘自Wiki)