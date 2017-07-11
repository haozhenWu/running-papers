# Modeling Industrial ADMET Data with Multitask Networks

Steven Kearnes

Brian Goldman

Vijay Pande

# 1 Introduction

2012 Merck Molecular Activity Challenge促进了很多人开始使用DNN解决drug discovery和cheinformatics,尤其是virtual screening.后续工作表明了神经网络比传统的机器学习方法都要更有效率.

Merck Challenge的获奖者使用了multitask neural networks.如果在multitask中,tasks相互之间有联系,那么parameters就可以有效地表达更多隐藏的信息.这种"data amplification"效应帮助解释了为什么multitask可以优于singletask.(也就是所说的multitask效应)

尽管multitask能帮助提升性能,但是并不清楚对于某一个数据集,究竟是什么因素能带来提升.一个主要的问题就是relatedness关联度并没有很好的定义.比如,Ma发现如果数据集上有许多想死的分子,那么multitask的性能会更加差.另外,用于训练multitask的数据量不确定,还有不确定的就是增加数据和tasks是否会提升性能.有一些使用公共数据的研究表示了增加大量的数据来提升multitask的性能,给人一种印象就是数据越多越好.

除了multitask效应的不确定性,从publicity available数据转移到工业数据可能效果没那么好.处理数据的质量,最可能的问题是使用了random cross-validation,而不是temporal validation.Sheridan表示一般意义上random cross-validation会给过于乐观的结果,而temporal validation在处理公共数据的时候,因为缺少temporal metadata,而难以给出很好的性能.在这篇文章中,Ma使用时序上注释的工业数据,来表明neural network比random forest效果好.

这篇报告描述了single-task和multitask neural networks在absorption, distribution, metabolism, excretion, and/or toxicity (ADMET)实验数据,在Vertex Pharmaceuticals.主要目标是(1)比较multitask和single task,同时还有baseline,random forest和logistic regression.(2)比较temporal validation和cross-validation对模型的提升.(3)探究可能对multitask effect产生影响的因素.同时还考虑multitask中information leakage问题.

# 2 Methods

## 2.1 Datasets

我们的实验着重于分类性能在22个Vertex dataset上(ADMET),包括hERG inhibition,aqueous solubility,compound metabolism.实验数据分为'active','inconclusion'和'inactive'.

因为active/inactive比例严重失调,进行加权.使用训练数据计算active/inactive ratio(使用cross-validation的时候,则是使用整个数据集来得到这个ratio).这个权值在logistic regression和random forest中并没有使用.

模型使用temporal validation scheme:每一个数据都有一个时间戳,分成了training,validation和test set,是根据时序进行的.大概比例是7:1:2.为了进行比较,同时也是用了random cross-validation.

化合物是使用1024位ECFP表示.

## 2.2 Model training and evaluation

使用TensorFlow.用向前算法和ReLU作为activation function, batch normalizer,0.5 dropout, adagrad optimizer, learning reate 0.001, batch size是128.训练了1百万个step.输入样例被编码到了一种密集模式,对于每一个分子的训练样例,包含了特征,label和对应于每一个task的权重.对于没有activity的label,权重设置为0.在好几个NVIDIA Tesla K80 GPU.

每一个neural network都是基于下面五个框架之一:(1000)作为baseline.(2000,100)和(2000,1000)检查最后一层的size如何影响结果.Ramsundar 2015年的一篇paper提出假说,金字塔形的框架,同时只有很少和任务相关的参数能够避免overfitting,同时仍然提供很丰富的特征表达.(4000,2000,1000,1000)框架是Ma 2015年一篇paper提出的进行regression操作,(4000)这个比较宽的single layer用来代替多层hidden layers.

RF和LR都是用的sklearn.RF的参数和Ma 2015年的paper类似.n_estimators=100, max_features=1/3., min_samples_split=6.这些参数是为了特定的分子选择的,不一定适用于circular fingerprints.

性能评估用ROC AUC.training的时候使用了checkpoint,选择最好的checkpoint在test上进行预测.(类似我们使用的early stopping模型中的best model)

尽管AUC是推荐的评估方法,但是不是那么好用.它反应的是active和inactive的相对位置,但是制定决策的时候需要的是一个阈值来划分预测信心.那么基于ROC曲线的enrichment scores可以在这里使用.至于如何选择替代的评估方法,如何选择阈值就不在这里讨论.

## 2.3 Model comparison

选择median AUC作为评估方法.95%的置信区间作为sign test stat.95%的置信区间是Wilson score interval,评估一个模型比另外一个模型号多少.不包括0.5的置信区间表示模型之间差别很大.所以,sign test置信区间评估模型之间的一致性,median AUC评估的是影响程度.

# 3 Results

## 3.1 Model performance

训练了single-task neural network(STNN)和multitask neural network(MTNN).训练了两种MSTNN,一种是uniform weight,另外一种是task-weighted(W-MSTNN),它给每一个task分配权重,于这个task的training compound成反相关,这也使得每一个任务的权重之和几乎一样.(这一块有点晕)

结果展示在table 2中.MTNN和S-MTNN皮鞭比RF好.但neural network不如LR鲁棒.(2000,100)和(4000,2000,1000,1000)MTNNs和LR差不多,但是(1000),(4000)和(2000,1000)MTNN和W-MTNN都比LR好.

table 3比较了single task和multitask在相同框架(参数)下的评估.MTNN和W-MTNN,只有(1000)和(2000,1000)展示了有效的提升.而这两者相互之间的差别不大.

table A2展示的结果表明,几个模型之间的相互比较,没有一个模型是固定比其他模型都好的,也就是说性能是强烈依赖于数据.也就无法得出一个一般化的结果,除了唯一的显示(2000,1000)的MTNN比LR,RF和(2000,1000)STNN结果好.

## 3.2 Factors affecting multitask learning

有一些研究表明multitask平均来说回避single-task有提升.但是也包括了一些例子,在一些数据集上,只有很小甚至负面影响。这个section讨论一下直接或间接带来multitask影响的因素。前面两个部分是讨论我们的模型和别人的研究之间关系。剩下的是探究dataset大小和task relatedness。

### 3.2.1 Temporal validation vs. random cross validation

Sheridan展示了temporal validation可以给更加精确的评估，相比于random cross validation。random cross validation减少了covariate shift——训练数据和测试数据服从不同分布的趋势。在pharmaceutical data中，covariate shift更为常见。最差的情况下，temporal validation的training和test set会有完全不同的属性。而random cross validation则是使得training set的数据更加偏向于test set。这个方法在机器学习中常用，但是实际中效果不好。

为了计算covariate shift，我们计算了最大Tanimoto相似度。temporal和random cross validation的最大相似度展示在图中。

结果表明了random cross validation对RF，LR，STNN的效果都比较好。MTNN则比较鲁棒。

### 3.2.2 Individual dataset size

Fig 4展示了multitask可能在更大的数据上效果更差。和前面的研究一致：有时候更小的数据集更加准确。然后改变数据的size，W－MTNN和MTNN的性能还是类似。

### 3.2.3 Total amount of data

deep learning有许多参数需要tune。

在前期的工作，都表示如果使用大量的数据会提高学习效果。我们建立了一个side info dataset，同时使用internal Vertex和public data。大约有38M的数据点和550个tasks。按照前面的假设，我们能够观测到性能的大幅度提升。

Table A10表明了增加side info，W－MTNN能大幅度提高性能（AUC最高）。而且per－task的权重也在大部分结构中提升了性能。但是并没有同时提升MTNN和W-MTNN的性能。在MTNN上的研究表明，增加side info，MTNN的性能不变或者下降。

这些都表明了增加side info的效果是和数据相关。

### 3.2.4 Task relatedness

有可能是我们收集的数据太过diverse，导致简单地将它们整合在一起会引入competing training signal，对multitask effect有负面影响。为了验证这个假设，我们在不同的subset上测试multitask model。Erhan在2006年提出了，将两个labels之间对应共享的compounds之间的关系作为相似度。

Table A13表明了subset和full set训练的结果。subset的训练结果比full set是差一点，但也不能排除是因为tasks数目少而带来的影响。

这些结果支持了不考虑task relatedness的时候，数据越多，结果越好。和3.2.3的结果明显不一致。我们认为是multitask的性能提升会随着数据增加到达一个转折点，过了这个转折点，额外的数据不会帮助甚至下降。这个转折点依赖于数据集，并且和模型中其他的task关联度相关。

## 3.3 Information leakage in multitask networks

multitask有info leakage的问题，带来过于乐观的validation结果，哪怕使用的是temporal validation。info leakage发生在某一个task的training set和另外一个task的test set相关。这种相关性是不正常的，比如一个新的screening campaign会包含和前面canpaign中使用很类似的target。

考虑multitask使用temporal validation。如果数据分为training和test，有可能一个task的training data可能和另一个task的training data产生的方式类似。也就是说，相互之间的training和test set会有交互。这就带来了leaky info。random cross validation一直是leaky。而info leak带来的影响，理论上是和task relatednesss成正相关。

info leak可以通过模型构建在past和future的方法进行预防。也就是一个时间戳，把所有的training和test数据都分开。把这个策略叫做non-leaky temporal validation。

把non-leaky MTNN和前面的结果比较。表明了前面的结果是有info leakage，使得有些模型的AUC变高。STNN上，non-leaky比leaky提升性能，per-task weight在（4000，2000，1000，1000）上也能提升性能。但是median AUC并不总是一个方向改变。non-leaky和leaky之间模型的比较，并没有解释两种validation策略上有什么明显不同。

我们担心这些结果会特定适用于我们的数据集，在不同的data ranges，relatedness和数据量结果不一样。尤其是实验数据的时间分布会导致不同的training和test data。我们这篇paper使用的leaky validation避免了对于特定时间范围的敏感性，但是会有info leakage。

# 4 Discussion

大致表明了random cross validation过于乐观。但是和temporal validation，两个都在整体性能提升的趋势上一致。

使用大量的side information并不能显著提升性能。relatedness还没有被完全理解。

理论上来说，我们还并未确认一个通用的更有的方法或者结构，能够在任意数据集上给出一个很好的结果。建议最好的方法是根据数据集，建立特定的模型。尤其是production stage。

但是最近一切关于特征表示的方法比较乐观。比如neural graph fingerprints和molecular graph convolutions。

# Appendix
