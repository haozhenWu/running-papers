# LightRNN: Memory and Computation-Efficient Recurrent Neural Networks

Xiang Li, Jian Yang, Nanjing University of Science and Technology

Tao Qin, Tie-Yan Liu, MSRA

基本想法是使用一个二维数组来表示word embedding,每一个单词(i,j)是第i行第j列,两个向量的联合表示.针对于这种二维数组的embedding方式,提出了LightRNN算法.

# Intro

传统的RNN模型中,每一个单词都会先从one-hot向量映射到一个embedding matrix.然后为了做预测(比如下一个单词),输出层会通过另外一个embedding matrix映射到词库中的所有单词(理解为一个非常sparse的sigmoid).