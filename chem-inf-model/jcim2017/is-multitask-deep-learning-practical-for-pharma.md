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