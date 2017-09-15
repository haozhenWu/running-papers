# Understanding Black-box Predictions via Influence Functions

Pang Wei Koh, Percy Liang

# Intro

以往理解deep neural network的方法是模型如何做出预测，比如通过将一个简单的模型只fit一下本地数据点，或者打乱测试数据看预测会怎么改变。

这篇论文反过来，从模型的预测反过来追踪训练数据。