# Stability and Generalization

Olivier Bousquet, Andre Elisseeff

# Intro

敏感度定义为，当输入数据改变的时候，对应的输出数据的改变。

算法的随机性会在两个地方产生：抽样产生learning set和noise in the measurements。这里关注第一个，sampling randomness，并且因为改变输入数据的组成，从而使得算法输出的改变。这种sensitivity approach可以用来获取empirical 和 generalization error 差值的 bound。

