# Efficient Distributed Estimation of Inverse Covariance Matrices

Jesus Arroyo, Elizabeth Hou

# Intro

inverse matrix经常用于分析gene expression和brain imaging。尤其是当数据符合Gaussian时候，在inverse covariance matrix非0的边表示一个在Markov random field中有一条边。（也是通过Z-transform之后设置一个threshold）因此有研究价值。

这篇论我研究了一个方法，在数据spread不同机器上的时候，来预估sparse inverse covariance matrix。这里提出的方法是在communication上具有非常高的效率，每一个机器和central hub（交互中心）住需要一次communication。

# Distributed Inverse Covariance Estimation by Debiasing and Thresholding

假设每一个机器都有数据$$X_m \in \mathbb{R}^{n \times p}$$，一共有M个机器。文章中的一个observation就是一个data point。covariance matrix是$$\Sigma$$，inverse covariance matrix是$$\Theta$$。