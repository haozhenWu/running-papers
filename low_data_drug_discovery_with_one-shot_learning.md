# Low Data Drug Discovery with One-shot Learning

Han Altae-Tran, Bharath Ramsundar, Aneesh S. Pappu, and Vijay Pande

# Abstract

在drug discovery上ML最近展现了很强大的用处.但是有一个限制就是在大数据集上的训练效果比较好.我们展示了one-shot learning,在小数据集上也能进行ML.提供了一个新模型,residual LSTM embedding,当它和CNN结合的时候,极大地提升在小数据集上学习有意义的距离测度.这整个都作为DeepChem的一部分.

# Introduction

DNN的成功原因之一,在于通过大量输入数据,学习到复杂的特征表示.DNN也帮助更好地理解化学过程.比如,2012年的Kaggle比赛,Merck赞助的,获奖队伍用multitask DNN,比baseline random forest提升了15%.随后,就发现massively multitask network的确可以在性质预测上进行极大的提升.同时,其他人做了一些复杂的结构,用来从分子结构中处理和提取特征.graph-convolutional architectures把小分子作为无详图,并在此基础上用convolutional layer建立特征.和原先small-molecule学习特征算法不同,比如circular fingerprints,新的graph convolutional特征提取结构是可以学习的,意味着可以用于提升性能.

这些方法的限制之一是需要大量的数据.比如,massively multitask network通常使用上百万的数据进行训练.最近的工作展示了,random forest和simple deep-networks能够从几百个化合物中学习到有意义的化学信息,但是哪怕一百个化合物在标准的drug discovery campaign中也是资源非常紧缺的.

最近ML的一些应用还表明了,在一些情况下,non-trival的分类器可以通过很少的数据就能学到.这些方法通过在数据集上学习到一个在输入空间上有意义的距离测度.这个复杂的测度用了比较新的数据点,然后再预测这些新的数据点的性质.这些方法叫做'one-shot learning'单点学习.比如matching-network,学习到一个在图像上的距离测度,用这个测度达到接近人类精确度的字符识别.

我们的模型也是使用类似标准one-shot learning的数学原先进行drug discovery.标准的one-short learning侧重识别新的类,比如sample中只有一个长颈鹿的情况下识别另外一个.在drug-discovery系统中,就好比是在一个新的训练系统中预测分子的行为.

我们使用了一个新的deep-learning 框架,叫residual LSTM,是matching-network架构和residual convolutional network的修改版.这个框架支持学习复制的测度,在已有的和未知的分子之间交换信息.

另外我们使用了开元方法,都在一个叫做DeepChem库内.

# Method

## Mathematical Formalism

考虑multitask,有些task被保留进行one-shot learning.保留下来的data都是只有很少的数据,不足以学习一个有效的机器学习模型.目标是充分利用训练任务中已有的信息,在测试系统建立一个强分类器.

T个任务,数据集是$$S = \{ (x_i, y_i)_{i=1}^m \}$$, $$y_i \in \{0, 1\}$$.对于某一个给定的任务,可得到的数据点叫做"support set".目的是学习一个函数$$h$$,在support set上进行参数选取,预测任意一个$$x$$在同样的系统中是否active.

## Simple One-shot learning

deep one-shot learning system使用convolutional layer讲图像编码到连续向量.简单的one-short中,这些向量可以给knn分类.$$a(x,x_i)$$表示加权函数.就这么理解,$$x$$是test set，将要进行预测的，而$$x_i$$是training set，是已经训练的。那么对于任意一个新的$$x$$预测公式就可以这么定义：

$$h_S(x) = \sum_{i=1}^m \, a(x,x_i)y_i$$

函数$$a$$叫做attention mechanism。就是一个非负函数，$$a(x,x_i)$$，满足$$\sum_{i=1}^m a = 1$$，也就是在support set上的一个概率分布。这个分布的主要目的是为了反应support set和query x的相似关系。最终的预测可以解释为在这个attention分布下的期望值。

attention mechanism依靠两个函数，$$f$$和$$g$$，来吧输入实例映射到连续表示的空间内。以前的工作中，这两个函数都是convolutional，但在我们的模型里，都是graph-convolutional networks。$$f(x)$$和$$\{ g(x_i) \}$$之间的相似度用$$k(\cdot ,\cdot)$$来计算。比如k可以是余弦相似度，attention mechanism就可以简单的定义为正则化相似度：

$$a(x,x_i) = \frac{k(f(x),g(x_i))}{\sum_{j=1}^m(k(f(x),g(x_i))}$$

这个公式叫Siamese one-shot learning method.

## Attention Based Embedding

前面一章节在one-shot learning中,用了相互独立的$$f(x)$$和$$g(x_i)$$.所以特征的映射并没有用到support data的上下文信息.matching-network通过full context embedding来解决这个问题,ebmeddings是$$f(x) = f(x|S)$$,$$g(x_i) = g(x_i|S)$$.这使得$$x$$和$$x_i$$相互影响.根据经验，能提高one-short learning的结果.

为了构造$$f(x|S)$$和$$g(x_i|S)$$,matching network用标准的convolutional network计算了embeddings $$f'(x)$$和$$g'(x)$$.embedding $$g(x)$$是通过将$$g(x_i)$$放到向量中,然后将每一个元素通过bidirectional LSTM连接,每一个$$g(x_i)$$都受到$$g'(x_j)$$的影响.

$$g(x|S) = BiLSTM([ g'(x_1) | ... g'(x_m) ])$$

为了计算embedding $$f(x|S)$$,matching networks发明了一个context based attention LSTM模型(
attLSTM).这个模型计算的是一个和输入顺序无关的结合结果:

$$f(x|S)=attLSTM(f'(x), {g(x_i|S)})$$

BiLSTM和attLSTM都是讲向量序列变成一个向量的机制.但是attLSTM和顺序无关,BiLSTM是和顺序相关的.希望的是顺序无关,因为support set中instance出现的顺序是不确定的.而且$$f$$和$$g$$是非对称的,尽管$$g(\cdot|S)$$只依赖于$$g'$$,$$f$$依赖于$$f'$$和embedding $$g(\cdot |S)$$.

## Iteractive refinement using Dual Residual LSTMs

我们的架构中保留了context-aware的设计,但是解决了顺序相关的问题,和非对称的处理.

核心的想法是使用attLSTM来产生query embedding $$f$$和support embedding $$g$$.因为matching network需要$$g(\cdot|S)$$来定义$$f(\cdot|S)$$,解决这个问题的方法是迭代执行,同时计算.

第一次迭代时,$$f(x) = f'(x)$$,$$g(x)=g'(x)$$.然后使用attention mechanism迭代的更新$$f$$和$$g$$.这种构造运行embeddings迭代的表示另外一个.如下面的式子:

Initialize

$$r=g'(S)$$, $$\delta Z = 0$$, $$ \delta z = 0$$

Repeat

$$ e = k( f'(x) + \delta z, r)$$, $$e = k( r + \delta Z, g'(S))$$, similiarity measures

$$a_j = e_j / \sum_{j=1}^m e_{ij}$$, $$A_{ij} = e_{ij} / \sum_{j=1}^m e_{ij}$$, attention mechanism

$$ r = a^T r$$, $$ r = A g'(S)$$, expected feature map

$$\delta z = LSTM([\delta z, r])$$, $$ \delta Z = LSTM([\delta Z, r])$$, generate udpates

Return

$$f(x) = f'(x) + \delta z$$, $$g(S) = g'(S) + \delta z$$, evolve embeddings


## Graph Convolutions

标准的convolutional network中,每一个图都被当做grid,每一个点都被当做pixel.当计算某一个节点的convolution output时,这个节点的所有邻居都被传到一个密集的层级来形成这个节点的新特征.以此来发现一些本地特征.

标准的convolutional networks有pooling layers.我们用了类似的analogous max pool操作,返回每一个节点和它的邻居的最大activation.这种方法有利于增加convolutional layer而不增加参数的个数.

在graph-convolutional系统中,每一个节点都有一个向量的描述.但是在预测的时候,我们需要将整张图变成一个fixed size的单独向量.我们引入了一个新的graph-gather convolutional layer,将一张图内的所有节点的的特征向量加到一起,来得到一张图对应的特征向量.

这些方法都能帮助将小的分子转换到连续的向量表示.对应DNN有帮助.我们使用graph-convolutional neural net来把小分子进编码,适合one-shot预测.

DeepChem包含了graph-convolution, graph-pooling, graph-gathering.

## Model Training

Residual LSTM和matching networks类似,只是这里是multitask.$$Tasks$$表示training tasks,$$S$$表示support set,$$B$$表示query batch.

$$\theta = arg \underset{\theta}{min} \, E_{T \in Tasks} [ E_{S \sim T, B \sim T} [ \underset{(x,y) \in B}{\sum} log P_\theta (y | x,S) ]]$$

训练是有一系列episode组成,每一个episode中,任务$$T$$是随机抽样,然后support $$S$$和batch queries $$B$$ 从这个task内随机抽样.每一步gradient descent,使用ADAM优化.试着更多的gradient descent操作,然后发现在support内抽样更多,提升性能越好.

# Experiments

## Tox21

Tox21 有12个target,表示对人体的毒性.第一次聚集起来是为了Tox21 Data Challenge的比赛,获胜的队伍使用DNN.文章就是[DeepTox](https://arxiv.org/pdf/1503.01445.pdf).

作为low-data benchmark,我们没有使用这个数据集的train/set划分,而是使用前9个targets作为training,后面3个作为test collection.

## SIDER

SIDER包含了上市的药物和他们观测到的结果.最开始的时候测量了5868个有副作用的种类.但因为缺少这个粒度上的signal标记,我们讲drug-SE对并入到了27个system organ classes.把这个问题转化为了化合物对于某个target是否有副作用.(一共27个target)

## MUV

MUV包含了17个assays,用于virtual screening挑战.这个数据集的pos例子在结构上相互都是不一样的.所有这个数据集也是最好的baseline ml.对于low-data方法,则是'最差'的情况.因为所有的结构都是不一样的,在心的测试数据上无法找到结构上的类似.

## Transfer Learning to SIDER from Tox21

根据几个不同的数据集,实验也是从one-shot learning在closely related testing tasks转移到了更加一般化的情况.为了检查一般化是否有效,我们在Tox21上进行训练,在SIDER上进行预测.

# Discussion and Conclusion

主要贡献是提供了一个进行drug discovery的框架.DeepChem开源项目.

# Appendix

这篇还是Stanford Group发的,昨天刚刚发在[ArXiv](https://128.84.21.199/abs/1611.03199).