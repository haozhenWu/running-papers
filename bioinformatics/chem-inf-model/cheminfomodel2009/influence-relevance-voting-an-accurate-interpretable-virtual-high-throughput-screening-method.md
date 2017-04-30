# Influence Relevance Voting: An Accurate And Interpretable Virtual High Throughput Screening Method

S. Joshua Swamidass
Chole-Agathe Azencott
Tin-Wan Lin
Hugo Gramajo
Shiou-Chuan Tsai
Pierre Baldi

virtual high-throughput screening(vHTS)是经常被当作是一个classification问题，但也是可以被描述成一个ranking任务。

kNN是一个基于instance结构特征的方法。但前面的一些应用kNN的场景并未强调最近的neighbour贡献权重。应该是更加“近”的邻居，权重更大。kNN经常用来分类，而不是排序。kNN可以进行一些修改，输出的是[0,k]的数字，计算最近的k邻居中有多少是active。

这里提出了新的vHTS方法，使用IRV(Influence Relevance Voter)，类似于kNN拓展。

## Methods

要确定一种chemical similarity来定义chemical space的几何。使用fingerprint来作为特征。

使用SVM来评估和邻居的距离。

IRV就是使用neural network来结合化合物的邻居，来预测概率。预测是通过每一个邻居的 influence 来确定。influence是relevance乘以vote。图1给了一个算法的展示。

$$z(x) = \sigma(w_z + \sum_{i=1}^k I_i)$$

其中$$z(x)$$是输出的预测，$$I_i$$是第i个邻居的influence，$$w_z$$是bias。

而和上面所说的，influence的定义是$$I_i = R_i V_i$$，relevance和vote的乘积。

$$ R_i = \sigma ( w_y + w_s S_i + w_r r_i ) $$

其中$$s_i = S(X, N_i)$$是X和第i个邻居的相似度，$$r_i$$是根据相似度排名的第i个邻居的排名。$$w_y, w_s, w_r$$是对应的bias和参数。

$$ V_i = w_0 $$ if $$ c_i = 0 $$, $$ V_i = w_1 $$ if $$ c_i = 1 $$

其中$$w_0$$是inactive邻居，而$$w_1$$是active邻居。