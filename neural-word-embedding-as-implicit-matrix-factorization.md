# Neural Word Embedding as Implicit Matrix Factorization

Omer Levy, Yoav Goldberg

Bar-Ilan University

# Background: Skip-Gram with Negative Sampling(SGNS)

skip-gram模型假设有一个词库，和一个上下文库，分别用$$w \in V_W, c \in V_C$$表示。在Mikolov的几篇paper中，word来自于文本词库，context来自于大小为L的window，还有其他的context定义。我们将观测到的word和context pair标记为D。使用$$\#(w,c)$$来标记这个$$(w,c)$$对出现在D中的次数。类似的，有$$\#(w)$$和$$\#(c)$$。

考虑$$(w,c)$$对，让$$P(D=1|w,c)$$表示这一对来自于数据。那么有

$$P(D=1|w,c) = \sigma(\vec{w}, \vec{c}) = \frac{1}{1+e^{- \vec{w} \cdot \vec{c}}}$$

negative sampling用了一个比较tricky的方法，为观测到的的$$(w,c)$$最大化$$P(D=1|w,c)$$，同时为随机取样的negative examples最大化$$P(D=0|w,c)$$。因为有一个前提假设是为同样一个字，随机选取上下文会指向未观测到的$$(w,c)$$。因此SGNS的目标函数是

$$log \, \sigma(\vec{w}, \vec{c}) + k \cdot \mathbb{E}_{c_N \sim P_D}(log \, \sigma(- \vec{w} \cdot \vec{c} )) $$

