# cached long short-term memory neural networks for document-level sentiment classification

Jiacheng Xu, Danlu Chen, Xipeng Qiu, Xuanjing Huang

Fudan University

通过cache机制，来捕捉整体语义信息，将memory分成几组，对应不同的forget门，在文档集情感分析任务中取得了不错的结果。其实，RNN处理长文本信息都面临这个问题，chatbot中对context信息的处理也可以考虑借鉴这个思路。

# 1 Introduction

sentiment classification情感分类在很多领域中常用的NLP技术。

最近deep learning方法在sentiment classification取得了很大的进展，在feature engineering上取得了很大的成功。RNN是其中最常用的方法之一，因他可以处理不定长的字符串。

sentence- or paragraph-level的情感分析希望模型能够从有限的信息中挖取特征，而document-level的情感分析更多是从长文本中选择和存储全局情感信息，而长文本还伴随着噪音和重复的局部信息。简单的RNN无法处理溢出和选出关键情感信息。

很多试图解决在长文本上解决这类范围的限制，比如挖取情感层级。但是有些方法都是依赖于一个明确的prior structural assumptions或者舍弃同一个句子内的某些顺序信息。RNN模型模拟了人阅读的时候一个字一个字读的顺序，然后将句子之间的内部关系建立。通过控制字的顺序，RNN能够含蓄的挖掘句子的表示，同时分析整个文档的情感（不需要明确的边界）。

我们提出了Cached Long Short-Term Memory neural networks(CLSTM)来抓住大范围的情感分析。在dual store memory model中，memories可以住在短期记忆中缓存一段时间，同时加强它们和长期记忆的联系。CLSTM是给LSTM加了一个类似的缓存机制，内部内存被划分成了不同的部分，每个部分的forgetting rate都不一样。在我们的模型中，高的forgetting rate就扮演类似缓存的角色，把信息传送给lower forgetting rate的组。用不同的forgetting rate，CLSTM学会抓起、记忆和忘记长距离的情感信息。

我们的主要贡献如下：

+ 引人了一个缓存机制，设置不同的forgetting rate，实现不同的memory cycles，使内部内存多样化。我们的模型作为结果，能够抓住本地和全局的情感信息，也就更好的在长句子中实现RNN。
+ low forgetting rate的长期记忆，我们可以保存gradient在back-propogation过程中不变。因此我们的模型比LSTM converge更快。
+ 我们的模型在三个document-level的数据上比最新的方法都好。

# 2 Related Work

这个环节，主要以两个角度介绍相关工作：1. 现有的document-level情感分析 2. 提出一些LSTM的变形，这些变形都是用来解决存储长期信息。

## 2.1 Document-level Sentiment Classification

document-level的情感分类一直是情感分析中棘手的问题。最挑战的部分是文档中的每一个部分不是包含同样重要（可以用来推断情感）的信息，提出了很多方法，大部分都依靠传统机器学习西方发，需要人工修改特征。

最近基于神经网络的方法很受欢迎，因为它能够学习discriminative特征，Zhu和Tai在LSTM上使用了树的数据结构，来更好的进行语义组成；Bhatia通过使用额外的语篇模式结果提高情感分析。大部分这些模型在sentence-level或者paragraph-level上情感分类都很好，但是document-level，一个自底向上的层级策略会用来缓和模型复杂性。

## 2.2 Memory Augmented Recurrent Model

尽管这个观点已经广泛接受：LSTM比RNN有更长的记录单元，但是还是会忘记距离当前点比较远的信息。这个LSTM的范围性问题是将情感分析从sentence-level推广到document-level的关键。

不同的模型都被提出用来提高LSTM的延展性，来存储长范围的信息，两种方法还得到了关注。其中一个是用外部内存augment LSTM，但是在时间效率很低，因为耗用了大量的外部内存向量。和这个方法不一样，我们通过调整forgetting rates充分利用了LSTM的内部内存。

另一种方法是试着用不同的time-scale来区分不同的状态。它们将隐藏的状态划分到好几个组，每一个组都用不同的频率激活并且更新（比如有的每两个time-step更新，另外的每四个time-step更新）。这些方法中，不同的组完全连接，信息会从更快的组传播到慢的组，或者慢的到快的。

但是慢的组的内存不会在每一步都更新，导致情感信息丢失和不一致。在我们提出的CLSTM中，我们给不同的内存组分配不同的forgetting rates。这种新的策略使得我们可以在每一步都更新记忆组，而且更新时可以考虑到前一步所有的长短期记忆。

# 3 Long Short-Term Memory Networks

LSTM是一个经典的RNN，减轻了gradient的diffusion和explosion问题。LSTM可以抓住一个句子中的长期依赖，通过引入记忆单元和门机制，这两个都用来觉得如何使用和更新保存在记忆单元中的信息。

$$i^t = \sigma(W_i x^t + U_i h^{t-1})$$

$$f^t = \sigma(W_f x^t + U_f h^{t-1})$$

$$o^t = \sigma(W_o x^t + U_o h^{t-1})$$

$$\tilde c^t = tanh( W_c x^t + U_c h^{t-1} )$$

$$c^t = f^t \odot c^{t-1} + i^t \odot \tilde c^t$$

$$h^t = o^t \odot tanh(c^t)$$

$$\sigma$$是logistic sigmoid函数，$$\odot$$是元素对应的乘法操作，$$i^t$$,$$f^t$$, $$o^t$$和$$c^t$$分别是在t输入门、忘记门、输出门和记忆单元的激活向量，大小都是$$R^H$$。$$W_i$$, $$W_f$$, $$W_o \in R^{H \times d}$$是训练的参数。因此$$H$$和$$d$$分别是隐藏单元和输入的维度。

# 4 Cached Long Short-term Memory Neural Networks

LSTM能同时抓取长期和短期依赖，但是当处理长文本的时候，LSTM会无法获取关键信息。尤其是错误的信号会导致梯度消失，因此导致难以训练。

因为标准的LSTM无法避免得损失关键特征，我们提出了cached LSTM(CLSTM)来抓取长文章信息。而且，为了更好地控制和平衡历史信息和正在读入中的信息，我们采纳了Greff 2015年提出的LSTM的变体，Coupled Input and Forget Gate LSTM(CIFG-LSTM)。

**Coupled Input and Forget Gate LSTM** 以前的研究表明合并后的版本和标准LSTM性能几乎一致，同时使用输入门和遗忘门会导致冗余信息。

在CIFG-LSTM中，输入门和遗忘门被合成一个统一的门，$$i^t = 1 - f^t$$。我们用$$f^t$$来表示这一对门。索引$$c^t$$的表示可以替换成如下

$$c^t = f^t \odot c^{t-1} + (1-f^t) \odot \tilde c^t$$

图1给了一个解释性的比较，关于LSTM和CIFG-LSTM。

**Cached LSTM** CLSTM通过缓存机制挖取长距离的信息，将内存分到了好几个组，设置了不同的forgetting rate，当做不同的filters。

不同的组挖取不同范围的依赖，高的forgetting rates组是短期记忆，而低的forgetting rates是长期记忆。

我们将记忆单元分作K组$$\{G_1, ..., G_K \}$$，每一个组都包含内部记忆$$c_k$$，输出门$$o_k$$和forgetting rate遗忘率$$r_k$$。不同组的遗忘率压缩到了几个不同的范围。我们将LSTM进行如下修改：

$$r_k^t = \psi(\sigma(W_r^k x^t + \sum_{j=1}^K U_f^{j \to k} h_j^{t-1} )) $$

$$ O_k^t = \sigma (W_o^k x^t + \sum_{j=1}^K U_o^{j \to k} h_j^{t-1} ) $$

$$ \tilde c_k^t = tanh( w_c^k x^t + \sum_{j=1}^K U_c^{j \to k} h^{t-1} ) $$

$$ c_k^t = (1-r^t_k) \odot c_k^{t-1} + r_k^t \odot \tilde c_k^t $$

$$ h_k^t = o_k^t \odot tanh(c_k^t) $$

$$r_k^t$$表示第k个记忆组第t步的遗忘率。$$\psi_k$$是压缩函数，限制了遗忘率$$r_k$$在一定范围内。为了更好地压缩不同的组，它们的遗忘率压缩到不同的区域。压缩函数$$\psi_k(z)$$可以这么定义：

$$r_k = \psi_k(z) = \frac{1}{K} \cdot z + \frac{k-1}{K}$$，

where $$z \in (0,1)$$是通过logistic sigmoid函数定义，因此$$r_k$$的范围在$$(\frac{k-1}{K}, \frac{k}{K})$$之间。

直观来说，$$r_k$$接近0的时候，group k更偏向于长期记忆；如果$$r_k$$接近1，那么就偏向于短期记忆。因此$$G_1$$是最慢的，$$G_K$$是最快的。快的组就类似缓存，从快的租到慢的组传递信息。

**Bidirectional LSTM** Graves和Schmidhuber提出了Bidirectional 
LSTM(B-LSTM)，利用额外的向后信息，因此提高内存容量。

我们也在CLSTM中使用了双方向的机制，而文本中的字会接受两个方向上相关的信息。第k组forward方向是$$[\overset{\to}{h_k^1}, \overset{\to}{h_k^2}, ... \overset{\to}{h_k^T}]$$，从左到右；而backward的方向是$$[\overset{\leftarrow}{h_k^1}, \overset{\leftarrow}{h_k^2}, ... \overset{\leftarrow}{h_k^T}]$$，从右到左。

因此我们将给定的文本中每个单词$$w_t$$编码到

$$h_k^t = \overset{\to}{h_k^t} \oplus \overset{\leftarrow}{h_k^t}$$

其中$$ \oplus $$表示级联操作。

**Task-specific Output Layer for Doucument-lavel Sentiment Classification** 有了能够处理长文本的能力，我们能在文档中进行情感分析。

第一个组，最慢的组，应该保存长期记忆信息，我们只利用这个组的最后状态来表示文本。

然后一个完全链接的layer输出是一个softmax funcion，用于对于给定输入，预测不同class的概率，概率分布如下：

$$p = softmax(W_p \times z + b_p)$$,

$$z$$在CLSTM中是$$\overset{\to}{h_1^T}$$。

# Appendix

太巧了，昨天刚刚和小伙伴讨论LSTM对于长文本分析的欠缺之处，今天就看到人做了这个。

[arXiv链接](https://128.84.21.199/abs/1610.04989)

根据[微博状态](http://weibo.com/2678093863/EdnNKqFFt?type=repost)，是复旦NLP组邱锡鹏老师的工作。