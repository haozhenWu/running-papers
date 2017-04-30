# Multi-Agent Cooperation And The Emergence of (Natural) Language

Angeliki Lazaridou

Alexander Peysakhovich

Marco Baroni

# Intro

这篇paper是第一个介绍了multi-agent coordination communnication games.我们的游戏是延伸于[Lewis' signaling game，Lewis信号博弈](https://en.wikipedia.org/wiki/Lewis_signaling_game).

简单的说一下这个游戏,有两个玩家,发送者和接收者.首先整个空间/世界可以是有任意数量的状态组成.发送者有一组固定的信号可以发送给接受者.接收者观测到信号,但并没有观测到世界的状态,必须采取某些行动.对于每一个状态,有一个唯一对应的正确性,而发送者和接收者都偏好接收者能够对每一个状态采取正确的行为.这个博弈游戏考虑到是共同利益.

# General Framework

K个玩家,每一个有对应的参数$$\theta_k$$,必须行动的任务集合,交流协议$$V$$.