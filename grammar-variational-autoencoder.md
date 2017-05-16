# Grammar Variational Autoencoder

Matt J. Kusner

Brooks Paige

Jose Miguel Hernandez-Lobato

# Intro

生成机器学习模型,能够很成功的将数据表示为连续空间.

数据首先通常是字符串表示,这是因为类似LSTM,GRUs,CDNNs模型可以非常好地利用字符串序列.比如SMILES.

但是这种表达方式比较脆弱:字符串的稍微改动就会使得分子变化剧烈.而且根据概率decoder生成的SMILES,很可能是无效的.

为了解决这个问题,我们提出了一种 使用grammar直接将离散数据的结构 联合考虑进来.

给定一个grammar,每一个有效的离散对象都能够描述出从grammar出来的解析树.因此我们提出了grammar variational autoencoder(GVAE),直接从解析树encode和decode.