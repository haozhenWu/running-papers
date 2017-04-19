# Learning to SMILE(S)

Stanislaw Jastrzebski

这篇paper是去年2月份就发出来的,其实很多人都已经做过类似的事情.idea不难想,主要是及早提出了这个问题.

## 模型

比较RNNLM (recurrent neural network language model)和CNN,以及传统的分类器模型,比如NB,SVM,和RF.

## 特征

都是使用SMILES作为特征,RNN模式是经典的embedding,CNN则默认为图的输入.

RNN和CNN都是使用data augmentation,然后对于同一个化合物,不同的augmentation中取预测的平均值.

其余传统的模型都是使用n-gram来取特征.

## 结果

结果基于log loss.CNN的结果最好,RNNLM结果最差.但是算comparable结果.所以可以算promising work.

# Appendix

可以参考作者的[主页](http://kudkudak.github.io/).