# The Marginal Value of Adaptive Gradient Methods in Machine Learning

Ashia C. Wilson, Rebecca Roelofs, Mitchell Stern, Nathan Srebro, Benjamin Recht

# Intro

自从前几年提出了adaptive gradient methods，就开始非常流行，因为训练很快。

但是在参数的个数大于数据点的个数的时候，我们提出了在adaptive和non-adaptive的方法上，generalization的表现不一样。

本篇论文的工作主要有三个

+ 进行同样数量的hyperparameter tuning，SGD和momentum SGD比adaptive methods要好。哪怕是在两类方法达到同样的loss时，adaptive性能上还是差。
+ adaptive在训练的初期会收敛的更快，但是在测试集上的性能会趋于平缓/停滞。
+ 不管使用那种方法，都要进行同样多的tuning。

# 

# Appendix