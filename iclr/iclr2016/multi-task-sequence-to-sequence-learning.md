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

**autoencoder**和**skip-thought vectors**。skip-thought vectors是在连续的两个句子对上进行训练，从而能够学习到长范围的，会skip-thought的objective。唯一的要求就是训练数据必须要是有顺序的句子，比如段落。

## Learning

训练使用alternating training approach，也就是对于每一个task，进行固定的epochs，然后换到下一个task（也就是不同的language pair）。

# Experiments

在multi tasks中，选出一个作为reference task。

+ Large tasks + small tasks: 一个结果是，如果增加非常少量的parsing mini-batches，能够极大的提高翻译质量。

+ Large tasks + medium tasks: 当在另外一个task上训练很小一部分时间的时候，会比原任务有提升。比如5次image caption generation（medium），100次translation。

+ Large tasks + large tasks: 对于sequence model，larger networks会有一定的影响。

+ Multi-tasks + Unsupervised Learning: 想探究**autoencoder**能否帮助提升翻译性能。按照many-to-many的设定，增加两个单独语言的autoencoder，对效果有一定的提升，取决于mixing coefficient（迭代次数比例）。使用**skip-thought vector**方法，perplexity和BLEU scores给出相反的结果，前者增加，后者减少。猜测是因为skip thought改变了翻译任务的本质：它是用一个句子来预测另外一个句子。而autoencoder是将一个句子翻译成同一个语言的另外一个句子。

# Appendix

所以整个来说，还是共享同一个模型架构。但是迭代地进行参数训练。

而且和我们关注的virtual screening还不一样，它是将不同种类的task结合到一起。而VS本质还是multi-label。