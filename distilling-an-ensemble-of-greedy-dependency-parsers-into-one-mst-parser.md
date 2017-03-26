# Distilling an Ensemble of Greedy Dependency Parsers into One MST Parser

Adhiguna Kuncoro, CMU

Miguel Ballesteros, NLP Group, Pompeu Fabra University, Barcelona, Spain

Lingpeng Kong, CMU

Chris Dyer, CMU & Google DeepMind

Noah Smith, UW-Seattle

引入了两种基于图的一阶依赖性解析器。使用neural network的依赖性解析器已经实现目前最好的结果。但训练他们会在non-convex objective function进行gradient descent。对于一些任务来，使用不同初始化的neural network进行ensemble能够用来提升效果。

ensemble虽说能够提升准确度，但是并不是一个实际的解决方案，因为如果ensemble N个模型，就要跑N遍。我们解决方案是把ensemble提取到单独一个的FOG解析器。

## Notation and Definitions

first-order graph-based(FOG，也叫做"arc-factored")依赖解析器解决了

$$\hat y(x) = arg \underset{y \in T(x)}{max} \underset{(h,m) \in y}{\sum} s(h,m,x)$$

其中$$T(x)$$是基于$$x$$的有向树集合，$$s$$是本地scoring function，而且是仅针对于某条边的。这个可以用最小生成树求解。

以前的研究大部分是使用ensembling neural network，从不同的起始点，比如machine translation和constituency parsing。

## Distilling the Ensemble

为了解决前面说的，N个模型进行ensemble的方法，我们引入了新的方法来"distilling"提炼到一个单独的解析器。前面已经有研究，是把一些baseline的解析器的输出scores作为特征，进行训练。这个方法优势在于，test新的数据时候，不需要向stacking和beam search一样，对新数据都应用一遍N个parsers。这里提到了Hinton在2015年关于Distilling的一些用法。可以将来看一下。

## Appendix