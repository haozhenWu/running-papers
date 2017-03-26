# Not Just A Black Box: Learning Important Features Through Propagating Activation Differences

Avanti Shrikumar, Peyton Greenside, Anna Shcherbina, Anshul Kundaje

Standord

# Abstraction

神经网络一直就被认为是一个黑箱操作，需要对模型进行很好的解说。我们提出了DeepLIFT(Learning Important Feature)，一个有效且高效的计算神经网络importance score方法。

# 1 Introduction

神经网络的黑箱性质是理解模型的阻碍之一。理解特征的重要性可以带来重要的发现。提出了DeepLIFT方法。

# 2 DeepLIFT Method

如下定义：

$$C_{xy}$$：x对y的贡献

$$A_n$$：neuron n的activation

$$A_n^0$$：neuron n的reference activation。reference activation是对于某个应用，特定的reference input生成的activation。

$$A_n - A_n^0 = \delta_n$$

## 2.1 Summation to $$\delta$$

$$\underset{s \in S}{\sum} C_{sy} = \delta_y$$

## 2.2 Linear composition

$$O_x$$是neuron x的输出。

$$C_{xy} = \underset{o \in O_x}{\sum} \frac{C_{xo}}{\delta_o} C_{oy}$$

## 2.3 Backpropagation Rules

计算$$m_{xy}$$，定义为$$m_{xy} \delta_x = C_{xy}$$

推导得到$$m_{xt} = \underset{y \in O_x}{\sum} m_{xy} m_{yt} $$

## 2.4 A note on final activation layers

类似softmax或者sigmoid的activation函数，$$\delta$$最大为1.0。

# 3 Result

最有趣的是对于Genomics的实验。

使用DeepLIFT能够发现比gradient-based更多的重要特征。