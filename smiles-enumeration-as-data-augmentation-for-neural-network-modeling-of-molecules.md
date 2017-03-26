# SMILES Enumeration as Data Augmentation for Neutal Network Modeling of Molecules

E. J. Bjerrum

# Intro

在其他领域,比如图片分类和语音识别,data augmentation可以通过图片的旋转,镜像,变形,缩放来增加数据.

同一个分子有不同的SMILES(Simplified Molecular Input Line Entry System),而不同的SMILES可以用来data augmentation.(利用augmented data,相比于canonical SMILES,性能有提升.)丙烷有两种表达,甲苯有七种.

这里使用LSTM来使用SMILES评估.

