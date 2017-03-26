# Matching Networks for One Shot Learning


Oriol Vinyals, Charles Blundell, Timothy Lillicrap, Koray Kavukcuoglu, Daan Wierstra, from Google DeepMind

# Abstract

少量数据上，deep supervised learning的效果一般。这个框架是一个network，将少量标记的support set和未标记的样例映射到实际的label，而不用为了新的类别去调参。

# 1 Introduction

one-shot learning:从一个单独标记的模型学习。

deep learning的一个特点是需要大量的数据。增加数据和正则化一定程度上缓解low data的overfitting，但并未彻底解决这个问题。另外，在大数据集上学习的速度比较缓慢，使用SGD的话需要很多步更新参数。在我们看来，是因为参数化的模型，训练样本需要通过缓慢学习。

与此相反，许多非参数的模型可以让样例非常快速的同化。一些方法，比如kNN，不需要训练，而是需要定义一个metric。

将二者的优点结合，parametric model的泛化性，non－parametric model的速度快。我们模型的新颖地方在两处：模型层次和训练过程。我们提出了Matching Nets(MN)。然后再训练过程上，基于简单的机器学习原则：训练和测试条件必须一致。因此为了快速学习，每一个类别只训练很少的样例，不断选择新的minibatch，。。。

# 2 Model

## 2.1 Model Architecture

最近几年，许多组都增强neural network框架。有一篇文章将seq2seq转换成了set。[Order matters: Sequence to Sequence for sets](https://arxiv.org/abs/1511.06391)

我们的贡献是将one－shot learning转换成set－to－set框架。最重要的一点是，训练过后，Matching Networks能够对未观测到的数据进行预测，而不用改变network的结构。就是说，我们想把一个k个样例（很小）组成的support set，$$S = \{(x_i, y_i)\}_{i=1}^k$$映射到分类器$$c_S(\hat x)$$上，这个分类器能够给定一个测试样例$$\hat x$$，得到它在输出$$\hat y$$上的概率分布。定义$$S \to C_S(\hat x)$$为$$P(\hat y| \hat x, S)$$，而$$P$$在这里是由neural network定义。因此，当给定一个新的support set$$S'$$的时候，就使用$$P$$来对测试样例$$\hat x$$进行预测，$$P(\hat y| \hat x, S')$$。预测的结果就是$$ arg\, max_y P(y | \hat x, S)$$。泛化的结果是

$$ \hat y = \sum a(\hat x, x_i) y_i \tag{1}$$

这个结果依赖于$$a(.,.)$$，attention mechanism。简单的可以用softmax函数，$$a(\hat x, x_i) = e^{c(f(\hat x), g(x_i))} / \sum e^{c(f(\hat x), g(x_i))}$$,$$f$$和$$g$$是embedding function，通常是适当的neural networks。我们的实验中，两个都是深度卷积网络，或者是简单的语言任务的文字嵌入。

上面的式子(1)是判别式的。对于一个给定的support set，取样之后分类$$\hat x$$，完全有可能$$\hat x$$与某些pairs$$(x', y')$$充分重合，满足$$y' = y$$，而$$\hat x$$和剩下的pairs会有偏移。

我们提出了将整个集合$$S$$和$$x_i$$同时考虑在内的embedding function，$$g$$变成了$$g(S,x_i)$$。因此，g函数可以修改如何嵌入$$x_i$$。我们使用了bidirectional LSTM对集合S内地$$x_i$$进行编码。

同时使用整个集合上的read－attention LSTM：

$$f(\hat x, S) = attLSTM(f'(\hat x), g(S),K)$$

$$f'(x)$$是通过CNN训练得到的特征，作为LSTM的输入，在每一个时间戳内都是常量。$$K$$是固定LSTMunrolling的步数，$$g(S)$$是我们希望attend的集合。

## 2.2 Training Strategy

前面的Matching Network就是将一个support set映射到分类函数，$$S \to c(\hat x)$$，最终得到的映射结果是$$P_\theta(.|\hat x, S)$$。

每次训练就是抽样出$$S$$和$$B$$，然后在$$S$$上训练，在评估$$B$$上的预测错误，来调整参数。

$$\theta = arg \underset{\theta}{min} \, E_{T \in Tasks} [ E_{S \sim T, B \sim T} [ \underset{(x,y) \in B}{\sum} log P_\theta (y | x,S) ]] \tag{2}$$

$$S' \sim T'$$，在新的label分布式也能学习很好。严格地说，因为这个模型本质是non－parametric，所以不需要在没有遇到过的数据集上也有fine－tuning。很明显的，当$$T'$$和$$T$$偏离比较严的时候，模型工作不会很准。

## 3 Related Work

[Neural Turing Machine](https://arxiv.org/abs/1410.5401)

[Memory Networks](https://arxiv.org/abs/1410.3916)

# Conclusion

提出Matching Network的动力：1.训练一个network，使用one－shot learning更简单。2.non－parametric的结构对于neural network来说，更容易记录，并且应用到新的数据集上。此外，Matching Network不需要很昂贵地计算gradient。但是如果有outlier，这个模型表现不是很好。

# Appendix