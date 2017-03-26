# Generating Focussed Molecule Libraries for Drug Discovery with RNN

Marwin HS Segler, Thierry Kogej, Christian Tyrchan and Mark P Waller

## Abstract

主要展示了可以用RNN生成模型来生成分子结构。

## 1 Introduction

## 2 Methods

### 2.1 Representing Molecules

通常分子是由图结构建模，通常叫做Lewis结构。在这个模型中，原子是结点，bonds是连边。化学分子模型都是用图来表示的。但是在自然语言处理中，输入和输出通常是序列，比如字母、字符串或句子。因此使用SMILES结构。

### 2.2 Language Models and Recurrent Neural Networks

给定一个单词顺序，语言模型预测第$$i+1$$个单词的分布。语言模型既可以抓住语法的准确性，又可以抓住句子的准确含义。语言模型经常用在信息自动校正上。

可以这么表达

$$P_\theta(S) = P_\theta(s_1) * \prod_{t=2}^T P_\theta(s_t|s_{t-1}, ..., s_1)$$

使用RNN来计算这个概率。

这里提出来一个概念，RNN进行训练的时候，上一层隐藏层到下一层，可以通过一个recursive函数定义。但是也可以通过一个unrolled network定义。unrolled就是隐藏层$$h_1 \to h_2 \to h_3 \to ...$$。

这里使用LSTM作为具体的RNN函数。使用"one-hot"表示法，将SMILES符号作为输入。

### 2.3 Transfer Learning

数据集小弟时候，许多机器学习模型会overfitting。transfer learning在这种情况下有所帮助。

首先一个模型在大数据集上，在另一个task进行测试。然后在小的数据集上重新测试，叫做fine-tuning。目的是为了学习到大数据集上的整体特征，可能在第二个task上面有所帮助。为了产生focussed分子库，先在大数据集上（一般化的分子集合）进行训练，然后在小集合上对某个特定分子进行fine-tuning。这之后进行sampling。

### 2.4 Target Prediction

为了确定产生的模型在目标target上是否active，用机器学习的方法学习一个分类器来判断分子对于目标target是否active。通常使用一个threshold来划分分子是否active。$$pIC_{50} = -log_{10}(IC_{50})$$是比较常用的。$$IC_{50}$$是half maximal inhibitory concentration, the concentration of drug that is required to inhibit 50% of a biological target's function in vitro.

使用ECFP4（fingerprint with radius 4）作为描述符特征，然后用random forest，logistic regression，（Deep）Neural Network和gradient boosting tree进行评估。发现GBTs模型比其他的稍好。

### 2.5 Data

使用ChEMBL数据集合中1.4 M的数据。对应有72 M的字符，和51个唯一的字符。这也就意味着生成的新的分子也只包含这51个字符。

### 2.6 Model Evaluation

为了预测测试集上的模型$$T$$，和通过sampling，模型产生的分子集合$$G_N$$，计算$$\frac{|G_N \bigcap T|}{|T|}$$和enrichment over random(EOR)。EOR是这么定义的：$$EOR = \frac{\frac{n}{|G_N|}}{\frac{m}{|R_M|}}$$。其中$$n=|G_N \bigcap T|$$是从测试集T中随机采样$$G_N$$集合，通过fine-tuned生成模型重新生成的分子。$$m = |R_M \bigcap T|$$，则是从测试集T中随机采样$$R_M$$集合，通过unbiased生成模型重新生成的分子。EOR在直观上反映了fine-tuned模型与一般化模型相比的优势。

## 3 Results and Discussion

主要解决了两个问题：首先，产生大量的分子来进行virtual screening campaign。用RNN模型在一个一般化的大模型上训练，来学习SMILES语法。这个模型能够生成很多unfocused分子。为了解决第二个问题，生成focussed的分子，生成目标target的可能active药物分子，使用transfer learning:选择一小部分目标target的active分子，然后进行refit。每一次epoch之后，从模型采样来产生新的actives。另外，探究是否transfer learning对模型有正面影响，和没有pre-training(RNN)的模型进行比较。

### 3.1 Training the recurrent network

使用三层LSTM的RNN，每一层都是1024长，后面用0.2的drop out层。用batch size是128。

### 3.2 Generating Novel Molecules

有847,955个有效且唯一的分子。使用内部AstraZeneca过滤器来检查生成的化合物是否是有效的。

### 3.3 Generating Active Drug Molecules and Focused Libraries

大约四次epoch之后，50%生成的分子都被预测是active的。

通过RNN随机产生的分子和测试集大约有25%的重合率。

### 3.4 Simulating Design-Synthesis-Test Cycles

整个模型的流程：使用100k个unbiased样例训练语言模型（LSTM），然后使用训练的分类器（GBT或者叫target prediction model）选出'active'的样例。然后重新用于训练RNN模型。RNN模型每5个epoch之后，抽样10k个分子。然后每次抽取的10k个分子通过分类器再次过滤，选出的'active'会添加到前一轮（这里的round指的是什么？），结束循环。

### 3.5 Why does the model work?

如果只是生成新的分子对于特定的target active并没有帮助。因此我们通过高度EOR指数来表示。

## 4 Conclusion

LSTM可以大量生成和训练数据相似的分子。也就是这个可以用来生成分子库。而且我们也展示了transfer learning对于某一个特定的target，在只有一小部分active分子的情况下进行fine-tuned，使得能够创建所需要的active分子。

三个主要特点：

1. 生成模型与分类器分离
2. 模型很容易搭建
3. 将structure generation和optimization融合到了一个模型中

这个模型的缺点是可解释性。

## Appendix