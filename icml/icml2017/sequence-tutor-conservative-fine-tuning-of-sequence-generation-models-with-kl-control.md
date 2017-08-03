# Sequence Tutor: Conservative Fine-Tuning of Sequence Generation Models with KL-control

Natasha Jaques, Shixiang Gu, DzmitryBahdanau, JoseMiguelHernande-Lobato, Richard E. Turner, Douglas Eck

# Intro

使用RNN进行序列生成是比较常用的想法。这篇paper进行了改进：一个RNN先是通过在数据上进行MLE训练，得到了一个概率分布：给定序列的历史，生成下一个token的概率。另外一个RNN使用RL生成更高质量的输出。这里是使用了KL-control off-policy方法。

使用MLE进行训练RNN一直都有问题，比如在生成的序列上，noise会有叠加影响：前一个时刻的generated text shift，会影响到后面时刻。

这里提出了Sequence Tutor，通过task-specific reward，来使得RL和sequence generation RNN结合起来。