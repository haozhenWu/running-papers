# Molecular De-Novo Design through Deep Reinforcement Learning

Marcus Olivercrona, Thomas Blaschke, Ola Engkvist, Hongming Chen

# Methods

## RNN

RNN使用cell的结构来存储过去的记忆。在序列的头和尾分别使用"GO" "GOS"来标记开始和结束。之后使用训练的RNN来产生新的序列。

SMILES有一些注意的地方：我们要表示的是atom-level，而不应该是character-level。"Cl","Br","nH"不能认为是两个atom，而应当是一个。

## Reinforcement Learning

考虑一个agent，给定状态集和行为集。想要预测$$\pi(a|s)$$是给定状态，预测行为发生的可能性。