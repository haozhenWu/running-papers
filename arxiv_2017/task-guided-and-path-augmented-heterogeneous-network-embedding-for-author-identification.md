# Task-Guided and Path-Augmented Heterogeneous Network Embedding for Author Identification

Ting Chen, UCLA

Yizhou Sun, UCLA

# Intro

heterogeneous network包含不同类型的node和link.比如,在bibliographic network(书目网络)中,节点类型有论文,作者,连边类型有 作者写论文, 论文包含关键字等.

这篇paper主要是挖掘hetegenerous bibliographic network的信息,更确切的说,是在双盲review的情况下,确定作者身份.

# Preliminaries

## Hetegenerous Networks

如前面所说,就是网络有不同的节点和连接.节点有论文,作者,关键字,地点,和年份.链接有 作者写论文,论文包含关键字.

## Meta path

