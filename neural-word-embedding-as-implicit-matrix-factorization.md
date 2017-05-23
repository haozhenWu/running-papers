# Neural Word Embedding as Implicit Matrix Factorization

Omer Levy, Yoav Goldberg

Bar-Ilan University

# Background: Skip-Gram with Negative Sampling(SGNS)

skip-gram模型假设有一个词库，和一个上下文库，分别用$$w \in V_W, c \in V_C$$表示。在Mikolov的几篇paper中，word来自于文本词库，context来自于大小为L的window，还有其他的context定义。我们将观测到的word和context pair标记为D。使用$$#(w,c)$$来标记这个$$(w,c)$$对出现在D中的次数。类似的，有#(w)和#(c)。

