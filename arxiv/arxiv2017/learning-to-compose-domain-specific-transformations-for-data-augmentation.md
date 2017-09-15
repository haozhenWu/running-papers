# Learning to Compose Domain-Specific Transformations for Data Augmentation

Alexander J. Ratner, etc.

# Modeling Setup and Motivation

以往augmentation的方法，会要求将每个instance augment之后，还能够保持同样的label比率。这个假设难以保证。

这里提出了一个弱一点但是更加可靠的假设 weaker invariance assumption，每一个instance augment之后，只要保证符合同样的label分布即可。

使用GAN来进行data augmentation。discriminator用来判断生成的数据是否在期待的label分布中，而generator用来生成instance sequence，同时让discriminator无法判别是否是真实数据还是生成数据。

生成器有两种类别：

**Independent Model:** 考虑mean field model，每一个transform function都是独立的选择。（也就是生成的序列不存在前后关系）这对于离散随机的transform function，学习特别方便。

**State-Based Model:** transform function之间也会有某些效应，比如同时的模糊和放缩处理会给数据带来损失。mean field model无法将这些考虑在内。因此，使用LSTM来对这些transform function加上序列关系。