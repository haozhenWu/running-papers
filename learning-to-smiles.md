# Learning to SMILE(S)

Stanislaw Jastrzebski

这篇paper是去年2月份就发出来的,其实很多人都已经做过类似的事情.idea不难想,主要是及早提出了这个问题.

## 模型:

比较RNNLM (recurrent neural network language model)和CNN,以及传统的分类器模型,比如NB,SVM,和RF.

## 特征:

都是使用SMILES作为特征,RNN模式.

其余的模型都是使用n-gram来作为特征.

## 结果

结果是

Based on results, (log loss), CNN is best, and RNNLM is worst.
I think we can argue that simply consider los loss cannot fully present the performance of a classifier. And we haven't trained other mentioned models in our case, except RNNLM.

[4:18] 
Also this paper applied data augmentation, but not into details.

# Appendix

可以参考作者的[主页](http://kudkudak.github.io/).