# Self-Paced Learning for Latent Variable Models

M. Pawan Kumar, Ben Packer, Daphne Koller

Stanford

# Intro

latent variable model 这种算法，很容易卡在了bad local optimum。为了解决这个问题，提出了对输入的数据进行排序可以缓解。

最大的挑战是我们没有给定一个很好的方法来确定样本的“容易”程度。我们的结局方案是使用iterative self-paced learning algorithm，每次迭代的过程选择容易的样本，从而训练一个新的参数向量。

# Preliminaries

