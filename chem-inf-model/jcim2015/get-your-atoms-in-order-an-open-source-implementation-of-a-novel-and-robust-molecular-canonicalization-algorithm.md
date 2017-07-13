# Get Your Atoms in Order - An Open-Source Implementation of a Novel and Robust Molecular Canonicalization Algorithm

# Intro

如何正确的表示一个分子，希望能够表示这个分子的结构，比如atom，bond，charge和stereochemistry。约25年前，Weininger发表了一个方法来生成canonical SMILES。但这个方法有一个问题，就是没有考虑到stereochemistry。

有一种index的方法，是graph relaxation algorithm。在cheminformatics，最常用对atom进行标记的方法是Morgan algorithm。