# DeepCCI: End-to-end Deep Learning for Chemical-Chemical Interaction Prediction

Sunyoung Kwon, Seoul National University
Sungroh Yoon, Seoul National University

提出了一个DeepCCI (compound-compound interaction),在七个评测方法中,比单纯的deep classifier都要好.

# Intro

在最开始的drug discovery中, deep learning并不是用于分类器,而是作为hidden feature extractor.

# Background

end-to-end模型特别依赖于输入的数据，需要能够包含化合物的很多特征.SMILES就能做到.

CNN在一维的生物序列和语言序列上,在二维的图像处理上,已经有了非常显著的效果.CNN包含了convolutional layer和pooling layer.在每一层convolutional layer上,都有特征映射,叫做filter,搜索所有的sub-region,然后产生特征.我们在SMILES上,使用CNN来获取特征.

# Proposed Algorithm

简单地说,这么几个步骤
1. input preprocessing: 首先将输入的两个化合物的SMILES,分别使用one-hot encoding.然后再padding.
2. modeling of hidden representations: 使用一维的CNN对SMILES进行特征提取.
$$ h = Pool(\sigma_r(Conv(x))) $$
3. modeling of interactions between chemicals: 将两个化合物的隐藏状态直接拼接,组成新的一维向量,在一个fully connected network上进行训练.

# Appendix

end-to-end model:传统的机器学习方法,需要我们进行人工提取特征,然后进行预测训练.但是在end-to-end模型中,将特征提取和预测模型整合到了一起.

其实简单的理解,不再在意专门的特征提取,而是由神经网络在训练的时候同时实现特征提取的目标,就叫做end-to-end.