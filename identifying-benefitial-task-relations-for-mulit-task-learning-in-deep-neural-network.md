# Identifying benefitial task relations for multi-task learning in deep networks

Joachim Bingel, Anders Sogaard

University of Copenhagen

# Intro

multi-task的优势是能够减少训练数据的需求,并且作为正则项使得模型更robust.已经有一些理论保证了在某些情况下,multi-task learning能够起作用.

# Multi-task Learning

在deep learning中, hard parameter sharing是非常常见的.有点是 (1)很常见的regularizer (2)容易实现.

multi-task经常被当做matrix regularizer,每一个model对应一行.常见的训练方式叫做mean-constrained learning,是使用matrix mean当作每一个模型的参数.

## Models

NLP的multi-task侧重于,使用基于RNN的sequence labeling.我们使用双向LSTM作为一层单独的hidden layer,维度为100,在所有task之间都共享.输入是100维度的GloVe embedding.

在我们的MTL设定中,每一个训练step都是单独唯一的task.这种训练方式很有意思,以往都是multi-task同时训练,这次是每次iteration,单独训练一个single task,这样就没有我们在multi-task中的对feature matrix missing data问题.

## Tasks

1. CCG Tagging(CCG): sequence tagging问题.
2. Chunking(CHU): 确定语义单元在连续空间的token,比如名词或者动词短语.
3. Sentence Compression(COM): 使用Google Compression dataset.
4. Semantic frames(FNT): 使用FrameNet 1.5来联合预测目标单词.
5. POS tagging(POS): 使用tweets数据集.
6. Hyperlink Prediction(HYP): 使用数据集hypertext corpus,预测括号里的单词序列.
7. Keyphrase Detection(KEY): 在科学发表论文中,检测关键短语.
8. MWE Detection(MWE): 使用Steusle corpus来确定multi-word expression.
9. Super-sense tagging(SEM): 使用标准的Semcor数据集划分,预测粗粒度的语义类型.
10. Super-sense Tagging(STR): 对于MWE任务,使用Streusle corpus,联合预测括号和粗粒度的语义类型.

# Analysis

根据表4,确定了最好描述MTL gain的是描述学习曲线的特征.一个重要的分析是:当main task的学习曲线比较平缓,而其他的辅助任务的学习曲线比较陡峭,MLT效果明显.换句话说,multi-task gain会发生在目标target非常快的进入平稳,而辅助任务不平稳.