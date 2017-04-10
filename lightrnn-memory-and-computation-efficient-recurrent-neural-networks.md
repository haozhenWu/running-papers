# LightRNN: Memory and Computation-Efficient Recurrent Neural Networks

Xiang Li, Jian Yang, Nanjing University of Science and Technology

Tao Qin, Tie-Yan Liu, MSRA

基本想法是使用一个二维数组来表示word embedding,每一个单词(i,j)是第i行第j列,两个向量的联合表示.针对于这种二维数组的embedding方式,提出了LightRNN算法.

# Intro

传统的RNN模型中,每一个单词都会先从one-hot向量映射到一个embedding matrix.然后为了做预测(比如下一个单词),输出层会通过另外一个embedding matrix映射到词库中的所有单词(理解为一个非常sparse的sigmoid).

# LightRNN

LightRNN最关键的一步是对单词的表示使用2-Component shared embedding.把整个词库的单词放到一个表中,分别有n个行向量和n个列向量,那么第(i,j)个单词就用i-th行向量和j-th列向量的组合来表示.

因此如图2显示,可以通过将basic units加倍来构造LightRNN.比如行/列向量是n,hidden state vector的维度是m.为了计算$$w_t$$的概率，要使用到列向量$$x_{t-1}^c \in \mathbb{R}^n$$,行向量$$x_{t-1}^r \in \mathbb{R}^n$$, 隐藏状态向量$$h_{t-1}^r \in \mathbb{R}^m$$.行/列向量是从table中获取,而隐藏状态函数通过如下更新:

$$h_{t-1}^c = f(W \cdot x_{t-1}^c + U \cdot h_{t-1}^r + b )$$

$$h_t^r = f(W \cdot x_{t}^r + U \cdot h_{t-1}^c + b) $$

其中W和U都是affine transformation.

所以对于预测的单词$$w_t$$,预测它的位置$$P(w_t)$$是通过计算列位置$$P_r(w_r)$$和行位置$$P_c(w_c)$$决定:

$$P_r(w_t) = \frac{exp(h_{t-1}^c \cdot y_{r(w)}^r)}{\sum_{i \in S_r} exp(h_{t-1}^c \cdot y_i^r) }$$

$$P_c(w_t) = \frac{exp(h_t^r \cdot y_{c(w)}^c)}{\sum{i \in S_c} exp(h_t^r \cdot y_i^c)}$$

$$P(w_t) = P_r(w_t) \cdot P_c(w_t)$$

