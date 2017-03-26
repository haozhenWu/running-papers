# A Simple But Tough-to-beat Baseline For Sensentence Embeddings

Sanjeev Arora, Yingyu Liang, Tenguy Ma

Princeton

# Intro

word embeddings能够获取单词之间的相似度。现在的工作试图让embeddings来获取word sequence的语义。

这里提出了一个非常简单的sentence embedding方法：仅仅计算每一个word vector的加权平均，然后通过PCA去掉第一个（最主要的）成分。每一个单词$$w$$的权重是$$a/(a + p(w))$$，$$a$$是参数，$$p(w)$$是估算的词频。这个叫做smooth inverse frequency(SIF)。

加权的结果比不加权的效果提升明显，也同时比一些supervised结果要好。

证明了如何使用max likelihood，来得到SIF reweighting。同时word2vec也是达到了类似的加权效果。

# Method

先介绍一下Arora提出的一个latent variable generative model。这个模型将corpus generation作为一个动态过程，第t个单词在第t步生成。而这个过程是由random vector $$c_t \in \cal{R}^d$$ 的random walk驱动的。词库中的每一个单词$$w$$在discourse vector中都有对应的向量，这些就是latent variables。

discourse vector是表示正在谈论的内容，而discourse vector $$c_t$$和word vector $$v_w$$的内积就表示当前谈话和单词的关系。而在t时刻观察到单词w，就通过log-linear word production model给出
$$
Pr[w \text{ emmited at time t} | c_t] \propto exp(\langle c_t,v_w \rangle)
$$

这里discourse vector $$c_t$$ 采用 slow random walk，也就是$$c_{t+1}$$是由$$c_t$$随机进行替换向量，所以临近的单词都在非常相似的对话环境下产生。

## Improved Random Walk model

通过Arora在16年的paper，已经如果使用MAP进行估计值，那么discourse vector就是word embedding的平均值。

这里考虑更加实际的问题（因为类似冠词没有意义，但是出现频繁的词）。通过加权来"smoothing term"。

$$p(w)$$是词频，$$\alpha$$是一个scalar参数。定义一个普通discourse vector $$C_0$$，用来当作一个高频率对话的纠正项。那么，给定一个discourse vector $$C_s$$，单词$$w$$出现在句子$$s$$中的概率是：

$$
Pr[w \text{ emmited in sentence s } | S_s] = \alpha p(w) + (1 - \alpha) \frac{exp(\langle \tilde C_s, v_w \rangle)}{Z_{\tilde S_s}}
$$

其中$$\tilde C_s = \beta c_0 + (1 - \beta) C_s, c_0 \bot C_s$$，$$\alpha$$和$$\beta$$是scalar参数。正则项$$Z_{\tilde C_s} = \sum_{w\in V} exp(\langle \tilde C_s, v_w \rangle)$$。

一个单词$$w$$和$$C_S$$无关但还能出现的情况有两个
+ 因为$$\alpha p(w)$$
+ 因为$$C_0$$。这里可以把$$C_0$$就当作一些包含类似定冠词的常用discourse。

## Computing the sentence embedding

假设单词$$v_w$$是随机分布的，因此partition function在任意方向大致是一样的，所以能够有如下的似然函数：
$$
p[s | C_s] = \prod_{w \in s} p(w|C_s) = \prod_{w \in s} [\alpha p(w) + (1-\alpha) \frac{exp(\langle v_w, \tilde C_s \rangle)}{Z}]
$$

然后求出句子的log-likelihood的导数
\frac{(1-\alpha)}{\alpha Z p(w) + (1-\alpha)}
$$
\bigtriangledown f_w(\tilde C_s) = \frac{1}{\alpha p(w) + (1-\alpha) \frac{exp(\langle v_w, \tilde C_s \rangle)}{Z}} \frac{1-\alpha}{Z} exp(\langle v_w, \tilde C_s \rangle) v_w
$$

所以根据泰勒展开

$$f_w(\tilde C_s) \approx f_w(0) + \bigtriangledown f_w(0)^T \tilde C_s = constant + \frac{(1-\alpha)}{\alpha Z p(w) + (1-\alpha)} \langle v_w, \tilde C_s \rangle$$

因此，$$\tilde C_s$$的MLE是

$$
arg \, max \sum_{w \in s} f_w(\tilde c_s) \propto \sum_{w \in s} \frac{a}{p(w) + a}
$$

其中$$a = \frac{1 - \alpha}{\alpha Z}$$。

也就是说，MLE大约是embedding words的weighted average。

# Appendix

中间一大段的公式推导本来已经写完，因为电脑内存不够crash了。这里我就不特地再写一遍了，paper里面说的已经很明确。

还有一个章节是和Word2Vec的关系，粗粗瞄了一下，将来再补上。