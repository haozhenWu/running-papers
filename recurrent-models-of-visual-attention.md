# Recurrent Models of Visual Attention

Volodymyr Mnih, Nicolas Heess, Alex Graves, Koray Kavukcuoglu

DeepMind

# Intro

CNN模型在图像识别上,有巨大的成功,但是对计算消耗要求比较高.提出了一个新的方法:使用attention-based task-driven的RNN.

# The Recurrent Attention Model(RAM)

我们把attention问题当一个goal-directed agent和虚拟环境交互的序列的决策过程.在每一个时间点,agent通过一个bandwidth-limited sensor来观测环境,比如它不会观测到所有的环境,它可以挖掘到local region或者narrow frequency band的信息.

agent可以主动地控制如何部署sensor资源.agent也可以通过实施操作来影响环境的真实状态.因为agent只能观测到部分的环境,所以他需要将信息按照时间整合起来,来确定如何部署操作以及如何高效地部署sensor.每一步,agent收到一个scalar reward,而agent的目标是最大化total reward.

## Model

**Sensor:** 能够通过一个bandwith limited sensor $$\rho$$,也就是一个patch,来获取partial image $$x_t$$.这里假设agent能够获取retina-like representation,$$\rho(x_t, l_{t-1})$$,从图片$$x_t$$中的位置$$l_{t-1}$$获取的信息.它对于$$l$$附件的区域使用高像素,其他区域用低像素,从而使得最终的输入维度降低.我们把这个lower resolution的表述叫做glimpse.

**Internal state:** 是对过去观测到的信息的总结;它将agent对环境的进行了编码,并且觉得下一步的操作以及如何deploy sensor.这个internal state是由RNN中的hidden layer表示.

**Actions:** 每一步有两种操作:一个是决定如何部署sensor(就是下面观察哪一个patch),以及一个环境的操作$$a_t$$.第一个location action是从location network参数化的分布$$f_l(h_t;\theta_l)$$中随机选择: $$l_t \sim p(\cdot | f_l(h_t;\theta_l))$$.而environment action是类似地从分布中获得$$a_t \sim p(\cdot | f_a(h_t;\theta_a))$$.

**Reward:** 采取了新的行动之后,agent获得了新的观测值$$x_{t+1}$$和reward信号$$r_{t+1}$$.

## Training

有三个network,如图一所示,有glimpse network,core network,和action network $$\theta = (\theta_g, \theta_h, \theta_a)$$.