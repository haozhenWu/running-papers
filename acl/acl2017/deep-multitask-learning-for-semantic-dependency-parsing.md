# Deep Multitask Learning for Semantic Dependency Parsing

Hao Peng, Sam Thomson, Noah A. Smith

# Intro

带上标记的有向图能够自然地表达语法。但是full semantic graphs要标记的话会非常贵，从而使得学习困难。

这里第一次将Semantic Dependency Parsing(SDP)作为试验台，并考虑multi-task训练。

这篇paper引入了一个新的系统，能够分别独立的将每一个formalism进行解析。使用bidirectional-LSTM加上multi-layer perceptron来对边（arc）进行评分，并进行预测。

然后展示两种multitask变形，能够隐含地将不同formalism之间进行建模。

# Broad-Coverage Semantic Dependency Parsing(SDP)

大致集中在三种semantic formalisms，每一个formalism的标记都是双方向的词汇关系（比如及物动词的主宾）。这三种formalism从不同的语言习惯中来，但是都为了抓住单词间predicate-argument关系。

semantic dependency更像是semantic role labeling或者是abstract meaning representation。形式上，就如图一，将每一个句子当作有向图，句子中的单词就好比是token。将semantic dependency表示成有向图有这么几个好处：

1. 更容易parsing
2. 表示更加自然，每一个单词/token可以是不知一个谓词的参数
3. 提供了一中whole-sentence analysis
4. 与syntactic tree相比，有更高level的nonprojectivity

预处理，将有环的图去掉，保证剩余的都是DAG。

**formalism（形式化）**：考虑三种形式化

1. DM(Delph-in MRS) 表达方式来自于DeepBank，是人工纠正的parse。来自于LinGO English Resource Grammar。
2. PAS(Predicate-Argument Structures) 表达方式来自于Enju Treebank，包含了来自于Enju HPSG parsser的自动parse。
3. PSD(Prague Semantic Dependencies) 表达方式来自于Prague Czech-English Dependency Treebank。

三个里面，PAS最贴近语法，而且最容易预测。PSD的标签最多，相应parser在上面的性能偏差。

# Single-Task SDP

basic model，每一个形式化的training和prediction都是独立的。

# Appendix

