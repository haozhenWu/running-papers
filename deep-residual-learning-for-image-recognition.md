# Deep Residual Learning for Image Recognition

Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, MSR

# Intro

已经被证明了,越深的网络,预测效果越好.但是深层的网络有一个缺陷,就是gradient vanishing/exploding.但是这个问题可以通过normalized initialization和中间normalization layer解决,可以处理几十层左右的NN.

而更深的网络,在开始converge的时候,会有degradation问题:随着网络的深度增加,准确度会饱和,然后急速下降.而且这并不是overfitting.

这篇paper提出来使用deep residual learning framework来解决degradation问题.