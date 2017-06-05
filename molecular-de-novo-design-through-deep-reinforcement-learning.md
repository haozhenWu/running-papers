# Molecular De-Novo Design through Deep Reinforcement Learning

Marcus Olivercrona, Thomas Blaschke, Ola Engkvist, Hongming Chen

# Methods

## RNN

RNN使用cell的结构来存储过去的记忆。在序列的头和尾分别使用"GO" "GOS"来标记开始和结束。之后使用训练的RNN来产生新的序列。

SMILES有一些注意的地方：我们要表示的是atom-level，而不应该是character-level。"Cl","Br","nH"不能认为是两个atom，而应当是一个。

## Reinforcement Learning

考虑一个agent，给定状态集和行为集。想要预测$$\pi(a|s)$$是给定状态，预测行为发生的可能性。而$$r(a|s)$$是reward。long-term return是对应于一个SMILES的$$G = \sum r_t$$

RL是这样的顺序，从一个状态进行一些操作，然后收到了rewards，policy是为了增加$$\mathbb{E}(G)$$。

第一个学习到的RNN在RL中的作用是提供了一个prior distribution：

# Appendix

同样使用RNN编码，但是用RL进行序列产生。

可以考虑在grammar tree上做RL。

但这个只是为了设计molecule，并没有判断的能力。