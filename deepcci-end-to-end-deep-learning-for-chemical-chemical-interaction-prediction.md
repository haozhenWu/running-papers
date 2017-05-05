# DeepCCI: End-to-end Deep Learning for Chemical-Chemical Interaction Prediction

Sunyoung Kwon, Seoul National University
Sungroh Yoon, Seoul National University

提出了一个DeepCCI (compound-compound interaction),在七个评测方法中,比单纯的deep classifier都要好.

# Appendix

end-to-end model:传统的机器学习方法,需要我们进行人工提取特征,然后进行预测训练.但是在end-to-end模型中,将特征提取和预测模型整合到了一起.

其实简单的理解,不再在意专门的特征提取,而是由神经网络在训练的时候同时实现特征提取的目标,就叫做end-to-end.