# Multi-task Neural Networks for QSAR Predictions

George E. Dahl, Navdeep Jaitly, Ruslan Salakhutdinov

主要是2012年Merck Molecular Activity Challange获奖队伍 使用multi-task NN内容介绍。

# Intro

Quantitative Structure Analysis/Prediction (QSAR)主要是研究这几个任务 ADMET (absorption, distribution, metabolism, excretion, and toxicity)


high-throughput screening (HTS)提供了真实数据 against assay

Quantitative Structure Analysis/Prediction (QSAR)主要是研究这几个任务 ADMET (absorption, distribution, metabolism, excretion, and toxicity)

neural network现在通常会训练多层，然后每一层多几千个hidden units。在小训练及数据上训练的时候，可以通过这些方法来避免overfitting： early stopping, weight penalties, unsupervised pre-training, dropout。

# Methods

提到了一些训练multi-nn的想法。