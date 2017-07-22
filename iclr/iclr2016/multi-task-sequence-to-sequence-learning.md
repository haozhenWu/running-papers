# Multi-task Sequence To Sequence Learning

Minh-Thang Luong, Quoc V. Le, Ilya Sutskever, Oriol Vinyals, Lukasz Kaiser

# Intro

seq2seq learning使用的是将不定长度的序列映射到不定长度的序列。最开始是在机器翻译中使用，但后来在图像主题生成image caption generation和句法成分分析constituency parsing中也是用的很好。这篇文章有三个部分

1. one-to-many：将一种语言翻译成多种，共享了encoder
2. many-to-one：多种语言翻译成一种，共享了decoder
3. many-to-many：多种语言翻译成多种语言，同时共享encoder和decoder

