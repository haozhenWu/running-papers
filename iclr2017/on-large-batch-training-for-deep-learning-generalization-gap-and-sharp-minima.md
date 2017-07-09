# On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima

Nitish Shirish Keskar, Dheevatsa Mudigere, Jorge Nocedal, Mikhail Smelyanskiy, Ping Tak Peter Tang

Northwestern University, Intel Corporation

# Intro

DL经常是解决一个non-convex的优化问题。mini-batch based SGD 可以理解为使用SGD，但加入了噪声。$$B$$通常远小于data point的个数，比如$$\{32, 64, ..., 512\}$$.

# Drawbacks of Large-Batch Methods

许多研究都发现了使用large-batch methods进行训练的时候，generalization gap会增加。