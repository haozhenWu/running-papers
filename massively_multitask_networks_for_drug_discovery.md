# massively multitask networks for drug discovery

Bharath Ramsundar

Steven Kearnes

Patrick Riley

Dale Webster

David Konerding

Vijay Pande

ArXiv 2015

# 1 Introduction

# 2 Realted Works

# 3 Methods

## 3.1 Dataset Construction and Design

模型在259个数据集上进行训练,主要是四个来源:PCBA,MUV,DUD-E,和Tox21.PCBA 128个,MUV 17个,DUD-E 102个,Tox21 12个.

注意的是没有对数据集进行preprocessing,比如移除人工的实验结果.

## 3.2 Small Molecule Featurization

使用了由RDKit产生的ECFP作为特征.molecule被分解到了碎片集合,每一个碎片都放射到周围邻居院子.每一个碎片都被分配了识别符,一个分子的识别符集合哈希到了固定长度的bit vector来构造fingerprint.ECFP4和其他的fingerprints通常在cheminformatics应用中用到,尤其是不同的化合物之间的相似度.许多分子没能featurize,因此没有在我们的模型中使用.

## 3.3 Validation Scheme and Metrics

传统的方法固定了training, validation和testing set.但是因为我们的dataset是highly skewed,所以导致性能会依赖于training/test split.为了弥补这个,我们用了stratified K-fold cross validation;每一个fold保存同样的active/inactive比例.

注意的是我们没有明确选择一个validation set.有一些dataset只有很少的一部分是active,而选择一个专门的validation set会skew结果.因此我们怀疑hypyerparameters的选择会受到information leakage的影响.但是我们的模型运行时并没有体现出对hyperparameter的选择高度敏感,所以我们假设information leakage在这里不是一个大问题.

然后根据cheinformatics的推荐,我们使用roc来评估模型.roc对于binary classifier就是画一个true positive rate和false positive rate比较的图.对于每一个dataset,感兴趣的是area under the 
ROC curve(AUC),一种评测分类性能一般化的方法.更一般化地,对于一个N datasets的集合,我们考虑mean和median K-fold-average AUC:

$$Mean/Medain \{\frac{1}{k} \sum_{k=1}^K AUC_k(D_n) | n=1,...,N \} $$

$$AUC_k(D_n)$$定义为在除了第k个fold上进行训练,然后在第k个fold上进行test的AUC.为了完整性,还使用了enrichment factor作为替代的evaluation metrics.

## 3.4 Multitask Networks

neural network是非线性的分类器,重复线性和非线性的转换.

$$x_{i+1} = \sigma (W_i x_i + b_i)$$

在L次这样的转换之后,最终network的layer回传给一个简单的linear classifier,比如softmax,预测input$$x_0$$是否属于$$label_j$$:

$$P(y_j|x_0) = \frac{exp(w^j)^Tx_L}{\sum_{m=1}^M exp(w^m)^Tx_L} $$

M表示可能的label,这里是2,$$w_j$$是每一层的weight vector.multitask network使用了N个softmax,每一个task一个.

# 4 Experimental Section

## 4.1 Experimental Exploration of Massively Multitask Networks

通过不同的hyperparameter来比较multitask network的性能,Tabel 2有一些结果.最好的结果是使用pyramidal multitask networks,相比于其他简单的模型,有了极大的提升,包括LR,RF,STNN,PSTNN.

每一个模型在DUD-E上的测试结果都特别的好,使得在这上面的比较没有太大的意义.因此我们将DUD-E排除在分析的结果外.但是在训练的时候,还是包括DUD-E,因为如果删除了会对结果产生负面的影响,我们从理论上说明了DUD-E对正规化分类器和避免overfitting有帮助.

最开始的探索中,有一个一贯的问题,就是overfitting.如3.1中所说,数据集中只有一小部分是positive.对于table2中单独的hidden layer,每一个dataset都与1200个参数相关.因为总共的positive只有几十或者几百个,在没有正规化的时候,overfitting会成为一个主要问题.

减小参数的个数是使用Pyramid的主要原因.在我们的模型中,第一层是比较宽的,有2000个,第二层比较窄,只有150个.这个维度上的下降类似GoogGLeNet中1\*1 convolutional结构.widehidden units使得我们提取出复杂而具有代表性的特征,而narrow layer则将这些特征使得这些特征针对于每一个task.将drop out设置为0.25有助于提升性能.我们也尝试了在每一个single task训练best pyramid network,来帮助理解我们模型是否有效.结果展示在Appendix中.

我们测试了结果对于hidden layer size的敏感度,分别是(1000,2000,3000)第一层和(50,100,150)第二层.在这些结构中,mean和median基本上都只在<.01的范围内波动,出了MUV的波动在0.38.发现performance对于learning rate和training steps的选择比较敏感.

## 4.2 Relationship between performance and number of tasks

前面展示了massively multitask network比single-task模型性能提升.在这里,我们试图理解为何multitask性能会随着增加task数量而提升.先验了解三个合理的增长曲线:

**over the hill**:性能先提升,然后下降

**plateu**:先提升,然后plateaus

**still climbing**:线提升很快,然后下调,但是下调的速率很低

我们尝试分别使用10,20,40,80,160和249个task,这些dataset都包含了十个固定的'held in'tasks,是随机选择了五个PCBA,3个MUV和2个Tox21.每一个选择的训练集都是这个preceding集合的超集superset,加上一些随机增加的新的任务.对于每一个set,都计算了平均的5-fold-average AUC.将这个整个过程重复随机进行了十次.也就是重复随机选了十个{10,20,40,80,160,249}个dataset序列.

Figure 3展示了我们实验的结果.基本上显示了AUC会随着任务数量的增加而增加,还是still climbing.也可能是因为我们的集合太小,能有一个替代的模型.

## 4.3 More tasks or more data?

这里试着研究一下total amount of data VS. total number of tasks.也就是说,许多tasks和少量相关数据,或者少量tasks大量相关数据,哪一个好?

我们构造了一个multitask network,分别是10,15,20,30,50和82个tasks.前面一个section中展示了它是有一个累积选择的,就是对于某一个特定的{10,15,...,82}序列,10个task都包含在15个task中,15个task都包含在20个中,以此类推.注意这个只在PCBA上训练.

然后将这个network在不同的data points上进行训练,分别是1.6M,3.3M,6.5M,13M和23M.进行sampling,对一个给定的任务,1.6M的数据一定会出现在3.3M的数据中,3.3M的数据一定会出现在6.5M中,想法和前面类似.决定使用更大的dataset.但是有一些数据是无法实现的,比如23M,因为没有这么多的数据.

Figure 4展示了一些实验结果.结果是两个都对performance有提升...

## 4.4 Do massively multitask networks extract generalizable features?

第一层layer挖掘的信息对许多任务都很有用.我们找到了一个将这种挖掘的方法应用到不在training set中的数据.在hold-out数据中的预测进行了描述.

结果在Fig 5中.许多数据都比hold-in的结果差,transfer learning的结果negative的也有.

另外值得注意的是,transfer learning的结果在更大量的数据集上结果更好,一种解释是更加一般化的做法,会受到missing data的影响.

## 4.5 When do datasets benefit from multitask training?

### 4.5.1 shared active compounds

active compounds比inactive更有信息量

###　4.5.2 Target Classes

集合所有的target都会通过multi-task training提升效果.

###　4.5.3 Duｐlicate Targets

multi task不会带来过多的info leakage．因此，有duplicate targets对结果影响不大.

# 5 Discussion and Conclusion



# Appendix

[ArXiv](https://arxiv.org/abs/1502.02072)