# Learning to Compose Domain-Specific Transformations for Data Augmentation

Alexander J. Ratner, etc.

# Modeling Setup and Motivation

以往augmentation的方法，会要求将每个instance augment之后，还能够保持同样的label比率。这个假设难以保证。

这里提出了一个弱一点但是更加可靠的假设 weaker invariance assumption，每一个instance augment之后，只要保证符合同样的label分布即可。

使用GAN来进行data augmentation。discriminator用来判断生成的数据是否在期待的label分布中，而generator用来生成instance sequence，同时让discriminator无法判别是否是真实数据还是生成数据。