# Deep Residual Learning for Image Recognition

Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, MSR

# Intro

已经被证明了,越深的网络,预测效果越好.但是深层的网络有一个缺陷,就是gradient vanishing/exploding.但是这个问题可以通过normalized initialization和中间normalization layer解决,可以处理几十层左右的NN.

而更深的网络,在开始converge的时候,会有degradation问题:随着网络的深度增加,准确度会饱和,然后急速下降.而且这并不是overfitting.

这篇paper提出来使用deep residual learning framework来解决degradation问题.

# Deep Residual Learning

## Residual Learning

传统都是拟合目标函数$$H(X)$$,但我们这里来拟合residual残差函数$$F(X)=H(X)-X$$.也就是,$$F(X)$$是我们这里学习的target,而$$H(X)=F(X)+X$$才是实际目标.

## Identity Mapping by Shortcuts

如图2所示,在residual network的building block中,最后一层就是$$H(X)=F(X)+X$$,然后再连接到某一个activation layer.而操作$$F(X)+X$$就通过shortcut connection和element-wise addition完成.如果$$H(X)$$和$$X$$的维度不一致，就通过一个映射矩阵$$W_S$$:

$$y = F(x, {W_i}) + W_S x$$