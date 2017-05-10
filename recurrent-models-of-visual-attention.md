# Recurrent Models of Visual Attention

Volodymyr Mnih, Nicolas Heess, Alex Graves, Koray Kavukcuoglu

DeepMind

# Intro

CNN模型在图像识别上,有巨大的成功,但是对计算消耗要求比较高.提出了一个新的方法:使用attention-based task-driven的RNN.

# The Recurrent Attention Model(RAM)

我们把attention问题当一个goal-directed agent和虚拟环境交互的序列的决策过程.在每一个时间点,agent通过一个bandwidth-limited sensor来观测环境,比如它不会观测到所有的环境.