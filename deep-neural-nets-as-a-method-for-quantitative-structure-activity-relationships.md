# Deep Neural Nets as a Method for Quantitative Structure-Activity Relationships

Junshui Ma, Robert P. Sheridan, Andy Liaw, George E. Dahl, and Vladimir Svetnik

# Intro

证明了在大多数情况下，DNN比RF性能更好。而且存在一套参数在大多数数据集上perform well。

# Methods

原来使用的descriptors是substructure descriptors，比如atom pair(AP)，MACCS pairs， circular fingerprints，而不是一些whole molecule descriptors。这篇paper同时使用了AP和DP descriptor(donor-acceptor pair)。

两种避免overfitting的方法：1. Hinton提出的generative unsupervised pretraining 2. drop-out。第一种方法是基于数据的regularizer。参数开始的时候初始化是随机化，但Hinton的方法是使用unsupervised learning进行初始化，而不考虑activities。

使用multitask DNN。

# Appendix

George E. Dahl就是2012年赢得Merk Molecular Activity Challange的一作。