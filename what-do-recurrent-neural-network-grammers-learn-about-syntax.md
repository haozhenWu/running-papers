# What Do Recurrent Neural Network Grammars Learn About Syntax?

Adhiguna Kuncoro, Miguel Ballesteros, Lingpeng Kong, Chris Dyer, Graham Neubig, Noah Smith

Recurrent neural network grammars(RNNG)是最近提出的概率生成模型。在语言模型和解析上展现了最新的结果。我们这里从语言学的角度探究他们到底学习了什么信息，忽略模型和数据，以及通过attention机制来增强RNNG。

## Recurrent Neural Networks Grammars

RNNG的开始设计是为了解决自然语句的句法推导进行的建模。

RNNG定义是$$\langle N, \Sigma, \Theta \rangle$$, $$N$$代表了non-terminal symbal非截止的符号，比如名词短语、动词短语；$$\Sigma$$代表了所有的符号；$$\Theta$$代表了模型参数。

RNNG基于一个抽象的状态机器，有一个stack存储部分完全的部件，一个buffer存储完整生成的部件，和一系列操作。为了生成一个句子$$x$$和对应的词组结构的树结构$$y$$，使用一些列操作从上到下构造。有三个主要的操作
+ NT(X):将一个非终止符号放入stack中，用一个开括号和非终止符号表示，比如"(VP"
+ GEN(T):产生一个终止符号，并把它同时放到stack和buffer中
+ REDUCE:暗示是一个闭括号")"，表示部件已经完成。所以组成这个部件的元素，都会从栈当中弹出，然后对这个部件使用composition function。

每一步都是用LSTM对stack，buffer和past actions进行编码，来确定下一步采取什么操作。

RNNG最核心的组件是composition function，是计算一个新的部件的vector representation，用到了LSTM。因为是生成模型，考虑最大化$$p(x,y)$$，公式如下

$$p(x,y) = p(a) = \prod p(a_t|a_1, ..., a_{t-1})$$

Vinyals也曾经提出类似的方法，但是composition function不一样，他用的是一个线性的方法。我们这里引入了LSTM，性能就大幅度提升。

## Gated Attention RNNG

现在试图理解组合词组表达的本质。和大部分neural network一样，解释composition function的行为很有挑战。幸运的是语言学理论提供了很多支持。

其中一个发现就是headness在词组表达中上演了核心作用。

## Appendix
