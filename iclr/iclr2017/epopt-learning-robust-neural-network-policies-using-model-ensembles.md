# EPOpt: Learning Robust Neural Network Policies Using Model Ensembles

Aravind Rajeswaran, Sarvjeet Ghotra, Balaraman Ravindran, Sergey Levine

# Intro

model-free algorithm，比如Q-learning, actor-critic, policy gradient都需要大量的learning times，尤其是和复杂的function approximation结合到一起的时候。而从是实际环境中获取samples更加复杂，因为从一个partially learned policy中sampling可能非常不稳定。因此model-free的deep RL方法通常需要大量的训练epoch。

model-based方法，就是使用某个模拟源来模拟真实的数据，能够使用simulated data，从而解决上面的几个问题。