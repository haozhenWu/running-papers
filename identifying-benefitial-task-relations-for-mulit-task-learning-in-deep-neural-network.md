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

## Tasks