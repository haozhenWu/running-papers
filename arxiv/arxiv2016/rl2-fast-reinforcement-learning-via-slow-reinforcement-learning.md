# RL^2: Fast Reinforcement Learning via Slow Reinforcement Learning

Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel

# Intro

现有的bayesian reinforcement learning是model－based方法，但限制太多。在最简单的情况下可以work，并且需要domain提供prior。

这里提出的一个想法是，把agent的学习过程本身当作objective，来学习一个distribution，反映了某种piror。

