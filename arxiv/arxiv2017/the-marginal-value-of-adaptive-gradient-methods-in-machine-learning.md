# The Marginal Value of Adaptive Gradient Methods in Machine Learning

Ashia C. Wilson, Rebecca Roelofs, Mitchell Stern, Nathan Srebro, Benjamin Recht

# Intro

自从前几年提出了adaptive gradient methods，就开始非常流行，因为训练很快。

但是在参数的个数大于数据点的个数的时候，我们提出了在adaptive和non-adaptive的方法上，generalization的表现不一样。

本篇论文的工作主要有三个

+ 进行同样数量的hyperparameter tuning，SGD和momentum SGD比adaptive methods要好。哪怕是在两类方法达到同样的loss时，adaptive性能上还是差。
+ adaptive在训练的初期会收敛的更快，但是在测试集上的性能会趋于平缓/停滞。
+ 不管使用那种方法，都要进行同样多的tuning。

# The perils of preconditioning

目标是为了证明：当有许多个local minima的时候，不同的算法会找到不同的结果。

non-adaptive methods会找到minimum norm solution。

adaptive methods可以构造出很多实例，最终是converge到 low $$l_\infty$$ norm，而非 low $$l_2$$ norm。

# Conclusion

证明都是基于classification问题。但是adaptive methods在GAN和Q-learning上都很受用，因为这些任务并不是解决optimization problem。