# Recurrent Recommender Networks

Chao-Yuan Wu, UT-Austin

Amr Ahmed, Google Research

Alex Beutel, Google Research

Alexander J. Smola, CMU

Hong Jing, LinkedIn

# Intro

传统的推荐系统假设用户或者电影的状态是静态（也就是忽略了时间性特征）。而这里提出的Recurrent Recommender Networks(RRN)能够预测将来的行为。

简单地说，就是现在的数据都是randomly split成training和test。但是实际中，training和test会有前后顺序。比如我在训练数据都是2.14之前的，测试集则都是2.14之后的，这样才能在实际运用中有意义。

这个解决方案就可以用latent variable模型来推断没有观测到的状态。

# Model

使用LSTM来获取用户和电影的时间依赖关系。为用户$$i$$和电影$$j$$的latent特征，用$$u_i$$和$$m_j$$来表示。那么更新函数（即LSTM的几个门）则为

$$
\hat r_{ij|t} = f(u_{it}, m_{jt})\\
u_{i,t+1} = g(u_{it}, \{ r_{ij|t} \})\\
m_{j,t+1} = h(m_{jt}, \{ r_{ij|t} \})
$$

其中$$\hat r_{ij|t}$$是在时间$$t$$时，预测用户$$i$$对电影$$j$$的评分，而$$r_{ij|t}$$是实际评分。

$$y_t = W_{embed}[x_t, 1_{newbie}, \tao_t, \tao_{t-1}]$$

其中$$x_t$$是时间$$t$$时的评分，$$1_{newbie}$$表示这个用户是否是新用户。最终embed生成的$$y_t$$，就是LSTM的输入。也就是

$$u_t = LSTM(u_{t-1}, y_t)$$



# Appendix

对于推荐系统一直是没有深入了解，这篇paper也只是看懂了问题，但是对于细节不甚了解。将来补上。
