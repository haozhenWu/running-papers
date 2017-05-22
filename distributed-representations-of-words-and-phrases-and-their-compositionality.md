# Distributed Representations of Words and Phrases and their Compositionality

Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg Corrado, Jeffrey Dean

# The Skip-gram Model

skip-gram的训练目的是为了找到单词的有效表达来预测上下文单词。训练的目标函数是log probability，文中的公式1。而最后的单词预测定义为

$$p(w_O | w_I) = \frac{exp({v'_{w_O}}^Tv_{w_I})}{\sum_{w=1}^W exp({v'_{w}}^Tv_{w_I})}$$

其中$$v_w$$和$$v'_w$$是输入和输出的word vector表示，$$W$$是词库的大小。因为词库大小太大，所以计算这个softmax的复杂度过高。

