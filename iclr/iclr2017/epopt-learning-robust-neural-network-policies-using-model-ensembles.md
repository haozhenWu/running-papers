# EPOpt: Learning Robust Neural Network Policies Using Model Ensembles

Aravind Rajeswaran, Sarvjeet Ghotra, Balaraman Ravindran, Sergey Levine

# Intro

model-free algorithm，比如Q-learning, actor-critic, policy gradient都需要大量的learning times，尤其是和复杂的function approximation结合到一起的时候。而从是实际环境中获取samples更加复杂，因为从一个partially learned policy中sampling可能非常不稳定。因此model-free的deep RL方法通常需要大量的训练epoch。

model-based方法，就是使用某个模拟源来模拟真实的数据，能够使用simulated data，从而解决上面的几个问题。

这篇文章提出了Ensemble Policy Optimization (EPOpt-$$\epsilon$$)算法。大致分为两个阶段

1. 给定某个source(model) distribution，找到一个最优的policy
2. 从这个robust policy来收集数据，并且来调整distribution

另外这里为了体现robust，使用了adversarial training。也就是将那些performance比较差的sample point更多的进行训练，从而使得最终的generalize更好。

# Problem Formulation

source和target domain分别用$$M$$和$$W$$表示，目标是为了学习$$W$$的最优策略。假设在source domain存在某个分布$$D$$，能够拟合$$W$$。

# Learning Protocol and EPOpt Algorithm

和target domain进行交互的时候，使用round；和simulator交互的时候，使用episode。

每一个round，在当前模拟的source distribution上计算robust policy之后，和target进行交互。然后，使用通过target domain产生的数据来update source distribution。因此，每一个round，我们进行两次更新：一个是真的robust policy的$$\theta_i$$，另外一个是source distribution $$\psi_i$$；而关键步骤就是根据source distribution来更新robust policy，在根据target domain产生的数据更新source distribution。

# Appendix

