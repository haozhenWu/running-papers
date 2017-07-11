# The cornucopia of meaningful leads: Applying deep adversarial autoencoders for new molecule development in oncolog

GAN模型。同时训练一个生成模型G和判别模型D。生成模型可以获取数据的分布，判别模型判别数据是从训练数据还是G产生的。G的训练过程是为了最大化D出错的概率，这和以往最小化loss function的生成模型不同。因为GAN不需要一个特定的likelihood表达方式，所以说替代maximum likelihood的最好方法。

提出了使用deep adversarial model，主要是adversarial autoencoder，来区分和生成新的化合物。

## Results

Adversarial autoencoder(AAE)的输入是binary fingerprint和分子的log cocentration。AAE输出concentration和一个向量，向量是每一位bit的概率。在最中间的latent layer，也引入了一个neuron(GI)，负责给出Growth Inhibition percentage，负值表示在药物治疗后肿瘤细胞会减少。

将32个向量进行screen，对抗整个72m化合物组成的库。使用maximum likelihood函数对每一个32个向量选择10个hit最多的向量。最后一共有69个唯一的向量。

为了评估结果，使用了Pubchem BioAssay数据来检查化合物的生物相关性。

## Materials and Methods

### Design and training of the GAN

