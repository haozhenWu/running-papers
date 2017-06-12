# Stability and Generalization

Olivier Bousquet, Andre Elisseeff

## Intro

敏感度定义为，当输入数据改变的时候，对应的输出数据的改变。

算法的随机性会在两个地方产生：抽样产生learning set和noise in the measurements。这里关注第一个，sampling randomness，并且因为改变输入数据的组成，从而使得算法输出的改变。这种sensitivity approach可以用来获取empirical 和 generalization error 差值的 bound。

## Preliminaries

$$S^{\backslash i}$$表示去掉第i个element。

$$S^{i}$$表示替换第i个。

empirical error (or resubstitution estimate)定义为每一个data point的loss取平均。

leave-one-out error (or deleted estimate)定义为分别去掉每一个data point的loss取平均。（所以大约是empirical error的m-1倍。m个data point。

## Defining the Stability of a Learning Algorithm

### Hypothesis Stability

整个loss expectation被$$\beta$$ bound

### Point-wise Hypothesis Stability

每一个data point的loss expectation被$$\beta$$ bound

### Error Stability

对于任意的训练数据集，risk被$$\beta$$ bound

### Uniform Stability

对于所以训练集，无穷norm被$$\beta$$ bound

## Generalization Bounds for Stable Learning Algorithm



## Appendix

[wiki](https://en.wikipedia.org/wiki/Stability_%28learning_theory%29) 也提到了这篇paper的一些内容。