# SMILES Enumeration as Data Augmentation for Neutal Network Modeling of Molecules

E. J. Bjerrum

## Intro

在其他领域,比如图片分类和语音识别,data augmentation可以通过图片的旋转,镜像,变形,缩放来增加数据.

同一个分子有不同的SMILES(Simplified Molecular Input Line Entry System),而不同的SMILES可以用来data augmentation.(利用augmented data,相比于canonical SMILES,性能有提升.)丙烷有两种表达,甲苯有七种.

这里使用LSTM来使用SMILES评估.

## Methods

### SMILES enumeration

使用RDKit产生SMILES.首先分子内的原子随机打乱,然后转换成RDKit molfile格式.然后重复这个一定的次数,来组合成一个新的SMILES string.

### Molecular dataset

使用DHFR inhibition data.按照1:9分成test set和training set.使用SMILES进行拓展,并且padding length是74.使用one-hot encoding向量化.使用log IC50和unit variance和mean zero进行正则化.

### LSTM neural network

多层LSTM层,batch mode,输出是single linear output neuron.使用Bayesian optimization,通过Gaussian processes,在Python package GpyOpt包中提供.

一个使用canonical SMILES,一个是用SMILES enumeration作为特征.

## Results

对于每一个canonical SMILES,增加约130倍的enumerated SMILES.