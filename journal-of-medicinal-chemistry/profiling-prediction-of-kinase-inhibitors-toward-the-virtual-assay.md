# Profiling Prediction of Kinase Inhibitors: Toward the Virtual Assay

Benjamin Merget, Samo Turk, Sameh Eid, Friedrich Rippmann, Simone Fulle

## Material and Methods

使用三个数据集，Proprietary, Open 和 这两个数据集的整合。

后续的chemical space使用PCA进行分析。

两种特征：ECFP和FCFP。然后对于每一个molecule，将这两个fingerprint进行拼接。

使用$$pIC_{50} = 6$$作为threshold。

使用RF和DNN。

RF使用2500个树，$$log_2(total\, feature\, num)$$。DNN使用两层2048的hidden unit，ReLU，0.5 drop out，batch normalization。