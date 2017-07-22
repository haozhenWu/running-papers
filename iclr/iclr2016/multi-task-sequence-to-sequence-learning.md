# Multi-task Sequence To Sequence Learning

Minh-Thang Luong, Quoc V. Le, Ilya Sutskever, Oriol Vinyals, Lukasz Kaiser

# Intro

seq2seq learning使用的是将不定长度的序列映射到不定长度的序列。最开始是在机器翻译中使用，但后来在图像主题生成image caption generation和句法成分分析constituency parsing中也是用的很好。这篇文章有三个部分

1. one-to-many：将一种语言翻译成多种，共享了encoder
2. many-to-one：多种语言翻译成一种，共享了decoder
3. many-to-many：多种语言翻译成多种语言，同时共享encoder和decoder

# Sequence To Sequence Learning

目前seq2seq的工作这些区别

1. architecture：undirected，bidirectional和deep multi-layer
2. RNN type：有LSTM和gated recurrent unit
3. input representation：早期是使用encoder state；现在提出来attention mechanism，大概是使用随机访问的记忆来处理长文本

# Multi-Task Sequence-To-Sequence Learning

按照前面说的三种设定，one-to-many，many-to-one，many-to-many。

## Unsupervised Learning Tasks

