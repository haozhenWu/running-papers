# Is Multitask Deep Learning Practical or Pharma?

Bharath Ramsundar, Bowen Liu, Zhenqin Wu, Andreas Verras, Matthew Tudor, Robert P. Sheridan, Vijay Pande

# Intro

说明了multi-task非常robust，并且相比于random forest，能够提供更好的性能。

使用四个数据集，Kaggle，Factors，Kinase，和UV dataset。

# Multitask Deep Learning with DeepChem

eval metrics包含了squared Pearson Correlation coefficient。

Kaggle和UV按照三比一分成了training和testing set，并且按照了时间顺序。Factors和Kinase使用neighbor-splits按照三比一的比率分成了training和testing set。

用了三种Network模型，Multi-task Network, Progressive Network,和Bypass Network。其他还有RF和single-task。

Progressive Network每次只更新一个column/task的weight。前面的task可能会参考，但是不用来更新。而且在task之间还会有一个隐藏的ordering，实际上并不存在这种ordering。

Progressive Network每个task之间相互独立，Multitask Network每个task之间share weights。所以使用Bypass Network将两个整合到一起。

使用squared Pearson correlation coefficient。过去的实验发现，multitask model并不是保证给一直的提升，(有些情况提升很大，有些反而有很多下降）。使用Pearson correlation coefficient能够帮我们比较相对于baseline method的提升。

这里提出了一个鲁棒的deep-learning结构能够一直比其他baseline methods好。

# Experimental Results

在Kaggle上，把RF当作baseline，multitask有11/15个更好，progressive，bypass，和singletask有9/15个task更好。

在Factors行，差不多都是4/12或者5/12个tasks。

其他都差不多。multitask是四个模型里面最好的。下面的表格是multi和single的比较。

| data | Improved |
| ---- | ---- |
| Kaggle | 8/15 |
| Factors | 11/12 |
| Kinase | 64/99 |
| UV | 155/190 |

# Appendix

这篇paper和Pande group去年暑假出的很类似。但是论文中省去了大量的细节，比如NN的hyperparameter，以及feature如何选取。