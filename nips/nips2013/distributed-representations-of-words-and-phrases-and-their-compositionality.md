# Distributed Representations of Words and Phrases and their Compositionality

Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg Corrado, Jeffrey Dean

# The Skip-gram Model

skip-gram的训练目的是为了找到单词的有效表达来预测上下文单词。训练的目标函数是log probability，文中的公式1。而最后的单词预测定义为

$$p(w_O | w_I) = \frac{exp({v'_{w_O}}^Tv_{w_I})}{\sum_{w=1}^W exp({v'_{w}}^Tv_{w_I})}$$

其中$$v_w$$和$$v'_w$$是输入和输出的word vector表示，$$W$$是词库的大小。因为词库大小太大，所以计算这个softmax的复杂度过高。

## Hierarchical Softmax

逻辑上是用一棵二叉树对output layer进行编码。物理上就是将每一个输出使用$$log_2(V)$$的binary vector进行编码。

定义函数$$ [x] = 1$$ 如果x为真，否则$$[x] = -1$$。

那么hierarchical softmax就可以定义为

$$ p(w|w_I) = \prod_{j=1}^{L(w)-1} \sigma([n(w,j+1) = ch(n(w,j))] \cdot {v'_{n(w,j)}}^T v_{w_I} )$$

中间的$$[n(w,j+1) = ch(n(w,j))]$$含义就是第j+1的预测是否为真。而且对于输出，不再是对每一个output word有一个mebedding，而是对每一个node有一个embedding $$v_n$$。

而整个的复杂度也降到了$$log_2(V)$$。

## Negative Sampling

如果不适用hierarchical softmax，可以考虑Noise Contrastive Estimation（NCE）。NCE有一个假设是，模型通过logistic regression能够将数据和噪声进行区分。

尽管NCE能大约最大化softmax的log probability，蛋SKip-gram模型只关心学习到的word representation，所以可以进行简化。定义Negative sampling的目标函数为

$$ log \, \sigma( {v'_{w_O}}^T v_{w_I} ) + \sum_{i=1}^k \mathbb{E}_{w_i \sim P_n(w)} [log \, \sigma(-{v'_{w_i}}^T v_{w_I} )] $$

来代替log probability。因此任务是使用logistic regression将目标单词$$w_O$$和从noise distribution $$P_n(w)$$中区分开来。其中每一个样本点，都有k个negative samples。

也就是把这一个output word和符合noise distribution取样的k个单词，组成一个小的batch，从而进行训练。假设原来最后一层输出层的搜索空间是$$ \text{embedding_size} \times V$$， 那么现在就是$$ \text{embedding_size} \times (k+1)$$。

# Appendix

[这篇博客](http://sebastianruder.com/word-embeddings-softmax/index.html#hierarchicalsoftmax)做了非常好的总结。