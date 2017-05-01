# Multi-Agent Cooperation And The Emergence of (Natural) Language

Angeliki Lazaridou

Alexander Peysakhovich

Marco Baroni

# Intro

这篇paper是第一个介绍了multi-agent coordination communnication games.我们的游戏是延伸于[Lewis' signaling game，Lewis信号博弈](https://en.wikipedia.org/wiki/Lewis_signaling_game).

简单的说一下这个游戏,有两个玩家,发送者和接收者.首先整个空间/世界可以是有任意数量的状态组成.发送者有一组固定的信号可以发送给接受者.接收者观测到信号,但并没有观测到世界的状态,必须采取某些行动.对于每一个状态,有一个唯一对应的正确性,而发送者和接收者都偏好接收者能够对每一个状态采取正确的行为.这个博弈游戏考虑到是共同利益.

# General Framework

K个玩家,每一个有对应的参数$$\theta_k$$,必须行动的任务集合,交流协议$$V$$.这篇paper主要关注在referential game上.

1. 有一组图形,表示为$$\{ i_1, i_2, ..., i_N \}$$，任意选取两张图片,分别叫$$(i_L, i_R)$$,其中一个被当作目标 $$t \in \{L, R\}$$.
2. 有两个玩家,发送者和接收者,每一个都看到了图像---发送者接收到了输入 $$\theta_S(i_L, i_R, t)$$.
3. 有一个词库V,大小为K,发送者选择一个,作为信号,发送给接收者.我们把这个称为发送者的政策 $$s(\theta_S(i_L, i_R, t)) \in V$$.
4. 接收者不知道target,但是知道发送者发送的信号,并且试着猜测目标image.我们把这个叫做接收者的策略,$$r(i_L, i_R, s(\theta_S(i_L, i_R, t)) \in \{ L, R \}$$.
5. 如果$$r(i_L,i_R,s(\that_S(i_L, i_R, t)))=t$$,也就是接受者猜到了目标,两个玩家都收到1的效益,否则就受到0的效益.

# Experimental Setup

介绍了数据集.20个大类,463具体的概念.从ImageNet中,每个概念选取100张图片.为了创建target pairs,随机选取两个概念,每一个概念对应选取一张照片,并确定哪一张照片作为目标.使用一个已经训练好的VGG ConvNet,