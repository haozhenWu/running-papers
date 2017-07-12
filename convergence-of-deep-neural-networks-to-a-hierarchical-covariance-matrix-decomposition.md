# Convergence of Deep Neural Networks to a Hierarchical Covariance Matrix Decomposition

Nima Dehmamy, Neda Rohani, Aggelos Katssaggelos

# Intro

DNN的训练很像物理模型spin glass。最小化loss function好比是在spin glass中找到ground gate（minimum energy configuration）。因为spin glass的energy landscape充满了local minima，所以优化问题是一个NP-hard问题。

提出了一个很有意思的观点，在低层的weight parameter是数据的covariance matrix的eigenvectors。

# ReLU and Compressive Sensing

ReLU相比于sigmoid和tan activation function有很多优势。而且只有weight／parameter have significant positive overlap with data，才会update。如果参数的数量远远小于数据的数量，那么就好比是compressive sensing。

# Solving

推导过程比较长，TBC

结论就是SGD过程就好比是迭代的KL(Karhunen-Loeve) transform。

然后试验部分就直接这么构建。把这个模型叫做Density Matrix Network(DMN)。

# Appendix

中间提到了KL(Karhunen-Loeve) transform好比是对数据进行MSE。附论文：K. Fukunaga, Introduction to statstical pattern recognition (2013)