# Implementation of the Hungarian Algorithm to Account for Ligand Symmetry and Similarity in Structure-Based Design

# Intro

ligand通常是小的organic molecule，receptor通常是target drug

# Theoretical Methods

先提出了两个没有基于atom种类，在optimization角度看就是不加任何限制，进行MSE。

后来提出了基于原子种类，使用Hungarian Algorithm的贪婪算法。从描述来看，是明显order-sensitive的方法。论文中描述的第二步就是寻找augment path的过程。

