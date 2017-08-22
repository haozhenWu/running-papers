# A Brief Survey of Deep Reinforcement Learning

Kai Arulkumaran, Marc Peter Deisenroth, Miles Brundage, Anil Anthony Bharath

# Intro

以往的RL受限于computation，只能处理low-dimension的数据。但如果引入了deep learning，就可以处理复杂的输入。

# Reward-Driven Behavior

RL的本质是通过interaction进行学习。一个RL agent会和环境进行交互，在收到一系列的observation之后，根据reward，改变自己的行为。

sequence action被reward终结。每一次进行state transition，都会得到一个scalar reward。agent的目标是为了学习到一个policy，能够最大化return。

# Appendix

收录于IEEE Signal Processing Magazine