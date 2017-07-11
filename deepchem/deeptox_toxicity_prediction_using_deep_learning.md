# DeepTox: Toxicity Prediction using Deep Learning

Andreas Mayr, Gunter Klambauer, Thomas Unterthiner and Sepp Honchreiter

提出了DeepTox pipeline.首先normalize chemical features.然后进行特征提取.下一步是ensemble.最终进行预测.

# 1. Introduction

毒性测试,如果用活体动物会有道德上的争议.ml就是一个很好的替代方法.

Deep Learning能够更好地构建新的,task-specific特征,这些特征使得DL优于其他计算方法.

# 2. Materials and Methods

## 2.1 Tox21 Challenge Data

给了12707个化合物,11764作为training,296个作为leaderboard,647作为testing.对应训练集,化合物的特征和12个targets(different toxic effects)实验结果已知.leaderboard set的label组织者在第一阶段没有提供,后期发布出来,这样参赛者能够根据leaderboard提升自己的模型性能.

## 2.2 Deep Learning for Toxicity Prediction

DL在机器学习上高度成功,改变了许多领域.我们使用DNN用了toxicity prediction,提供方法细节和算法调整方法.

### 2.2.1 Deep Neural Networks

介绍了ANN变深.基本就是DNN在每一层layer都有成千上万的neurons,ANN只有几个.一个neuron就是一个抽象的数值加上activation function.

### 2.2.2 Key Techniques for Deep Neural Networks

ReLU,rectified linear units,保证了spark的表达,并且抵消下降的梯度.

dropout,用来正则化.

cross-entropy,加上softmax或者sigmoid,在output layer层.

### 2.2.3 DNN Learning, Objective and Optimization

对最后的loss function进行加权,如果有值,active/inactive,则设置为1,否则为0.也就是忽略那些absence的数据.

### 2.2.4 Hyperparameter Settings and DNN Network Architectures

hidden units在{1024, 2048, 4096, 8192, 16384}中进行选择.

使用AUC作为early stopping.

### 2.2.5 GPU Implementation

使用了CUDA并行计算平台,NVIDIA Tesla K40 GPUs,相比于CPU能快20-100倍.

## 2.3 The DeepTox Pipeline

五个组成部分.数据前期处理,化学描述符(特征挖掘),构建模型(包括一些其他模型,如
SVM,Linear regression, random forest的ensemble method),模型评估(使用基于ECFP的Tanimoto相似度,cluster了五个fold),ensemble模型.

# 3 Results

## 3.1 Benefit of Multi-Task Learning

比较了single task和multi task training的结果.

## 3.2 Learning of Toxicophore Representations

neuron可能编码了toxicophore特征,为了检测DL是否真的够早了toxicophores,我们进行了独立的实验.

## 3.3 Comparison of DNN and Complementary Methods

比较了DNN,SVM,RF,ElNet(elastic net,就是linear regression).Table 4

## 3.4 Tox21 Data Challenge Results

赢了Tox21 Grand Challenge.

# 4 Discussion

提到了训练单个task的DNN只要10 min,和前一篇文章20M epoch相比,验证了我们的方法是可靠的.

# Appendix

[Front. Environ. Sci., 02 February 201](http://journal.frontiersin.org/article/10.3389/fenvs.2015.00080/full)

粗看一下,好像是介绍deep learning在tox上的应用,看看能不能来点启发.