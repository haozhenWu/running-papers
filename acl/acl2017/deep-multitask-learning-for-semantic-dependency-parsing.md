# Deep Multitask Learning for Semantic Dependency Parsing

Hao Peng, Sam Thomson, Noah A. Smith

# Intro

带上标记的有向图能够自然地表达语法。但是full semantic graphs要标记的话会非常贵，从而使得学习困难。

这里第一次将Semantic Dependency Parsing(SDP)作为试验台，并考虑multi-task训练。

这篇paper引入了一个新的系统，能够分别独立的将每一个formalism进行解析。使用bidirectional-LSTM加上multi-layer perceptron来对边（arc）进行评分，并进行预测。

然后展示两种multitask变形，能够隐含地将不同formalism之间进行建模。

# Broad-Coverage Semantic Dependency Parsing(SDP)

大致集中在三种semantic formalisms，每一个formalism的标记都是双方向的词汇关系（比如及物动词的主宾）。