# LightRNN: Memory and Computation-Efficient Recurrent Neural Networks

Xiang Li, Jian Yang, Nanjing University of Science and Technology

Tao Qin, Tie-Yan Liu, MSRA

基本想法是使用一个二维数组来表示word embedding,每一个单词(i,j)是第i行第j列,两个向量的联合表示.针对于这种二维数组的embedding方式,提出了LightRNN算法.

# Intro

传统的RNN模型中,每一个单词都会先从one-hot向量映射到一个embedding matrix.然后为了做预测(比如下一个单词),输出层会通过另外一个embedding matrix映射到词库中的所有单词(理解为一个非常sparse的sigmoid).

# LightRNN

## RNN Model with 2-Component Shared Embedding

LightRNN最关键的一步是对单词的表示使用2-Component shared embedding.把整个词库的单词放到一个表中,分别有n个行向量和n个列向量,那么第(i,j)个单词就用i-th行向量和j-th列向量的组合来表示.

因此如图2显示,可以通过将basic units加倍来构造LightRNN.比如行/列向量是n,hidden state vector的维度是m.为了计算$$w_t$$的概率，要使用到列向量$$x_{t-1}^c \in \mathbb{R}^n$$,行向量$$x_{t}^r \in \mathbb{R}^n$$, 隐藏状态向量$$h_{t-1}^r \in \mathbb{R}^m$$.行/列向量是从table中获取,而隐藏状态函数通过如下更新:

$$h_{t-1}^c = f(W \cdot x_{t-1}^c + U \cdot h_{t-1}^r + b )$$

$$h_t^r = f(W \cdot x_{t}^r + U \cdot h_{t-1}^c + b) $$

其中W和U都是affine transformation.

所以对于预测的单词$$w_t$$,预测它的位置$$P(w_t)$$是通过计算列位置$$P_r(w_r)$$和行位置$$P_c(w_c)$$决定:

$$P_r(w_t) = \frac{exp(h_{t-1}^c \cdot y_{r(w)}^r)}{\sum_{i \in S_r} exp(h_{t-1}^c \cdot y_i^r) }$$

$$P_c(w_t) = \frac{exp(h_t^r \cdot y_{c(w)}^c)}{\sum{i \in S_c} exp(h_t^r \cdot y_i^c)}$$

$$P(w_t) = P_r(w_t) \cdot P_c(w_t)$$

具体的iterative的预测是,给定第i-1的单词都列向量$$x_{t-1}^c$$,推测出第t个单词行分别$$P_r(w_t)$$,然后选择概率最大的行作为下一个单词$$w_t$$的行向量.

## Bootstrap for Word Allocation

关于如何构建table.

1. 采用冷起动,先随机将单词分配到表格中.
2. 使用table作为embedding vector来训练LightRNN,直到收敛.如果遇到stopping criterion,那么就退出.否则继续.
3. 根据前一步学习到的embedding vector,refine allocation,以减小loss function作为标准.

refinement allocation是其中关键一步.使用negative log-likelihood作为目标损失函数等同于对LightRNN模型的实际数据和预测结果进行的cross_entropy.给定T个单词,那么overall negative log-likelihood就可以写作:

$$NLL = \sum_{t=1}^T -log(w_t) = \sum_{t=1}^T -log(P_r(w_t) - log(P_c(w_t)) $$

如果假设行列的loss是可以分离的，那么可以得到

$$NLL = l_r(w,r(w)) + l_c(w,c(w)) $$

将一个单词$$w$$从$$(r(w),c(w))$$挪到$$(i,j)$$的所有可能性就是$$|V|^2$$.而所有的损失$$l_r(w,i)$$,$$l_c(w,i)$$在训练的过程中就已经计算完成.比如对于单词$$w$$,$$l_r(w,i)$$是该单词所有出现的$$-log \frac{exp(h_{t-1}^c \cdot y_i^r)}{\sum_k (exp(h_{t-1}^c \cdot y_k^r))}$$的总和.

# Experiment

使用perplexity(PPL)作为评测方式,定义为$$PPL=exp(\frac{NNL}{T})$$.

数据集是ACLW(ACL Workshop Morphological Language Datasets)和BillionW(One-Billion-Word Benchmark Dataset).

benchmark有两个,一个是HSM,使用hierarchical softmax作为预测;一个是C-HSM,同时使用hierarchical softmax用作预测,和character-level convolutionalfilters作为embedding.结果之一就是embedding size越大,越准确.