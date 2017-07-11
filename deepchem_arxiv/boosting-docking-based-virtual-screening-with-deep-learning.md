# Boosting Docking-based Virtual Screeening with Deep Learning

Janaina Cruz Pereira, Ernesto Raul Caffarena, Cicero Nogueira dos Santos

## Abstract

提出了一个提升基于docking的virtual screening方法。引入的deep neutral network框架，DeepVS，使用一个docking程序作为输出，并且学习挖掘相关特征。我们方法是用原子和氨基酸并实现了一个高效的分布式向量表达方式。

一个主要优点是不需要feature engineering。使用Useful Decoys\(UDU\)来评估DeepVS，使用两个docking程序的输出：AutodockVinal1.1.2和Dock6.6。使用严格的leave-one-out作为cross-validation，DeepVS在AUC ROC和enrichment factor上优于docking程序。

另外，使用AutodockVinal1.1.2的输出，DeepVS能够达到最高的AUC ROC 0.81，是目前为止知道最高的。

（开始使用enrichment factor作为评估方法）

## Introduction

## Materials and Methods

### DeepVS

DeepVS的输入是蛋白质化合物的结构描述，并产生一个score来区分ligands（配合基）和decoys（欺骗）。图一给出了细节。首先，给定一个蛋白质化合物x，信息是从原子中提取出来（第一次隐藏层）。一个原子的文本有基本的结构化数据组成，包括了距离、邻居原子的类型、原子的partial chages和乡里安定residues。下一步，从原子上下文中的每一个基本特征被转换成特征向量，并通过network被学习。然后，有一个convolutional层，被布置，用于总结所有原子的所有上下文的信息，并产生一个分布式的蛋白质化合物的向量表达r（第二层隐藏层）。最终，在最后一层（输出层），化合物的表达作为softmax分类器的输入，而这个softmax分类器会产生最后的分数。在算法一中，给出了一个高层次的伪代码实现。

### Atom context

首先，对蛋白质化合物进行一下基本的处理来挖掘DeepVS的数据。输入层使用每一个原子的上下文信息。原子"a"的邻居包括$$k_c$$最近的化合物内的原子（包括自己）和$$k_p$$个最近的蛋白质内的原子。而$$k_c$$和$$k_p$$是用户定义的参数。这种使用靠近某个原子最近的化合物和蛋白质的想法来自于structure-based drug design。

从原子中挖掘的基本特征包括原子种类、原子电子、氨基酸种类和邻居到参照原子的距离。

### Representation of the atom context
DeepVS的第一层隐藏层将每一个基本特征转换到实数向量（也就是平时熟知的embeddings），通过查找表格的操作。这些embeddings包含了通过神经网络自动学习的特征。对于每一个基本特征的类型，有一个对应的embedding matrix $$W$$，每一个列向量对应于每一个可能的数值。$$W^{atm}$$,$$W^{dist}$$,$$W^{chrg}$$,$$W^{amino}$$这四个矩阵包括了基本特征的embedding，分别从类型、距离、原子电子和氨基酸种类。这些矩阵组成了第一层隐藏层，并随即进行初始化。

$$W^{atm} \in R^{d^{atm} \times |A|}$$的每一列都对应一个特征向量，其中$$A$$是所有原子类型的集合，$$d^{atm}$$是embedding的维度。给定一个原子$$a$$的上下文，神经网络将每一个基本类型转换到对应的特征向量，并将这些向量一起组成了向量的原子类型表示$$z_{atm}$$。正如图三所示，从$$W^{atm}$$获取一个原子类型的embedding就是一个查找字典的操作。因此原子类型的embeddings顺序是随机的，并且对结果没有影响。但是原子类型的embedding的顺序组成的$$z_{atm}$$会对结构有影响。我们的做法是先按照原子种类从最近得到最远的，然后按照蛋白质种类从最近的到最远的进行拼接。

类似的，$$z_{dist},z_{chrg}, z_{amino}$$分别从目标原子上下文中的距离、电子和氨基酸种类出发构建。电子的数值和距离必须要先行离散化。离散化方法是取出最大和最小，按照0.05作为一格bin大小。

最终某一个原子$$a$$的特征$$z_a$$定义为$$\{ z_{atm}; z_{dist}; z_{chrg}; z_{amino} \}$$。我们的假设就是通过最基本的上下文特征，神经网络能够学习到更加抽象的特征（embeddings），包含了足够的信息来区分compounds和decoys。

### Representation of the Protein-Compound Complex

DeepVS的第二层是卷积层，负责提取更加抽象的特征，并且将信息总结到一个固定长度的向量$$r$$中。我们将$$r$$称作化合物－蛋白质复合表示，是卷积层的输出。

下一步的目标就是使用卷积层解决输入的不定长度问题。在virtual screening中，不同的化合物有不同的原子数目。因此，原子上下文的表示会因为上下文的不同而不同。在DeepVS中，卷积层允许不同的大小。

一个complex $$x$$，化合物是由$$m$$个原子组成，输入就是$$\{z_1,z_2,...,z_m\}$$，$$z_i$$是第i个原子的上下文表示。那么卷积层的第一步就是得到抽象:

$$u_i = f(W^1z_i + b^1)$$

卷积层的单元数量（也叫做filters）是用户定义的参数。

卷积层的第二步，也叫做pooling层，从不同的原子上下文中总结特征。输入是特征集合$${u_1,u_2,...,u_m}$$。DeepVS使用了max-pooling层，也就是产生了一个向量$$r \in R^{cf}$$，第j个元素定义为输入向量第j个中最大的，也就是

$$$
[r]_j = max_{1 \le i \le m} [u_i]_j
$$$

这一步产生的向量$$r$$是化合物-蛋白质复合物的表达。通过这种方式，神经网络学会如何产生一个向量表达，来从复合物中总结信息来区分ligands和decoys。

### Scoring of Conpound-Protein Complex

第三层的隐藏层就通过softmax学习到一个分数。

### Training DeepVS

使用SGD，配合minibatch。

### Experiment Setup

使用Directory of Useful Decoys(DUD)作为benchmark。

### DeepVS Hyperparameters

用leave-one-out来进避免overfitting。

#＃＃ Evaluation Metrics

ROC curves是一种表示selectivity(Se)和specifity(Sp)关系的方法。

$$

Se = \frac{\text{true pos}}{\text{total actives}}\\

Sp = \frac{\text{true neg}}{\text{total decoys}}

$$

enrichment factor at x% 则是表示，和随机选取的同等数目的化合物相比，预测分数最高的x%中的集合有多好。

$$

EF_{x\%} = \frac{\text{actives at x%}}{\text{compounds at x%}} \times \frac{\text{total compounds}}{\text{total actives}}

$$

## Result and Discussion



## Appendix

