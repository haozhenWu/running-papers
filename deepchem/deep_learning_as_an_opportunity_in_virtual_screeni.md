# Deep Learning as an Opportunity in Virtual Screening

Thomas Unterthiner, Andreas Mayr, Gunter Klambauer, Marvin Steigaert, Jorg K. Wegner, Hugo Ceulemans, Sepp Hochreiter

# Abstract

Merck Kaggle challenge won by Hinton影响很大。这里使用了一个比当时更大的数据集，ChEMBL，有13M的化合物描述，1.3M的化合物，和5k的targets。

# 1 Introduction

Kaggle比赛的数据并不够真实，所以将deep learning是否真的对drug discovery能产生影响还正在被发现。

# 2 Experiments

给定一个化合物$$i$$，预测是否在target $$t$$上active。是的话，label就是1；否则为0.同时预测m个targets。

## 2.1 Dataset

从ChEMBL dataset中得到一个prediction benchmark。ChEMBL将化合物存储为atom graph，可以挖掘出ECFP特征。目前最好的化合物描述法。因为targets是highly skewed，所以我们只保留了超过15个active compounds的target，也就是1230个targets。

性能通过AUC进行评估。

## 2.2 Methods

### 2.2.1 Deep Neural Networks

由一个或多个ReLU hidden units组成。使用Nvidia Tesla K40 GPU， 12GM内存来提速计算。但是为了适应大数据，multi-tas任务，对标准的DNN进行了一些修改。

**Multi-Task Learning** 框架是一层或多层ReLU hidden units组成，最后是一层1230 sigmoid output units，每一个target一个output unit。每一个训练样本只会对某几个task产生影响。因此在训练样例中并非active的输出单元，会在backpropagation中乘以$$\delta$$或者0.

因为有的task数据比较多，有点比较少，为了避免downweight smaller tasks，我们在hidden layer给每一个task加权，权重是available data points。

$$E(x_i, y_i, m_i) = - \underset{t}{\overset{T}{\sum}} m_{ti} \cdot w_t (y_{it} log (\sigma_t(x_i)) +(1-y_{it}) log( 1-\sigma_t(x_i) ) ) $$

其中$$w_t = \frac{1}{N_t}$$保证了在整个数据集上，每一个target都对hidden presentation有同样的影响力。

**Huperparameters** 我们用比较大的learning rate来弥补gradient的scaling influence。为了保证最后预测效应，使用cross validation。有43，340个输入特征，batch是1024，使用SGD进行训练。训练要花费3到4天。

## 2.3 Other Methods

**SVM**

**Binary Kernel Discrimination(BKD)** 使用density estimator来区分每一个target的active和inactive compounds。使用这个密度，对每一个待预测的化合物进行posterior probability预测。

**Logistic Regression**

**kNN**

**Paren-Rosenblatt**

**Pipeline Pilot Bayesian Classifier(PNPBC)**

**Similarity Ensemble Approach(SEA)**

## 2.4 Results

结果在Table 3中展示。

DNN结果最好。median AUC在0.8588。其中有12个实现了完美预测，（AUC=1.0）。

# 3 Conclusion

# Appendix

NIPS 2014