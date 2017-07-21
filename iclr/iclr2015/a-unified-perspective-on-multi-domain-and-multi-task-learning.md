# A Unified Perspective on Multi-Domain And Multi-Task Learning

Yongxin Yang, Timothy M. Hospedales

# Intro

我们提出了一个方法，能够替代zero shot learning：multi-task或者multi-domain是通过在不同的但是相近的任务或者领域之间共享知识。multi-domain指的是在不同的上下文domain中，对于同一个问题；multi-task指的是同一个domain解决不同问题。因为domain和task的气憋很低，这里不做区别。

# Model

考虑有M个domains/tasks，第i个domain有$$N_i$$个instances，第i个domain的第j个实例这么表示：$$\{ x_j^{(i)}, z^{(i)} \}$$，而特征就是$$y_j^{(i)}$$。