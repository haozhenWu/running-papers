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

IRV就是使用neural network来结合化合物的邻居，来预测概率。