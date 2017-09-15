# Learning to Compose Domain-Specific Transformations for Data Augmentation

Alexander J. Ratner, etc.

# Modeling Setup and Motivation

以往augmentation的方法，会要求将每个instance augment之后，还能够保持同样的label比率。这个假设难以保证。

这里提出了一个弱一点但是更加可靠的假设，每一个instance augment之后，只要保证符合同样的label分布即可。