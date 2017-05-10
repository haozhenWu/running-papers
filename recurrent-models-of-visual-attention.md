# Recurrent Models of Visual Attention

Volodymyr Mnih, Nicolas Heess, Alex Graves, Koray Kavukcuoglu

DeepMind

# Intro

CNN模型在图像识别上,有巨大的成功,但是对计算消耗要求比较高.提出了一个新的方法:使用attention-based task-driven的RNN.

# The Recurrent Attention Model(RAM)

我们把attention问题当一个goal-directed agent和虚拟环境交互的序列的决策过程.在每一个时间点,agent通过一个bandwidth-limited sensor来观测环境,比如它不会观测到所有的环境,它可以挖掘到local region或者narrow frequency band的信息.

agent可以主动地控制如何部署sensor资源.agent也可以通过实施操作来影响环境的真实状态.因为agent只能观测到部分的环境,所以他需要将信息按照时间整合起来,来确定如何部署操作以及如何高效地部署sensor.每一步,agent收到一个scalar reward,而agent的目标是最大化total reward.

