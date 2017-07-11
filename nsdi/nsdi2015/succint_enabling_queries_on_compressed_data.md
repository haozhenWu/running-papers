# Succint: Enabling Queries on Compressed Data

Rachit Agarwal, Anurag Khandelwal, Ion Stoica

UCB

在大量数据查询的时候，如果使用压缩作为优化方案，传统的压缩方法有trie树或者后缀树，但是对于内存的要求大概是10－20倍的输入数据。Burrows－Wheeler Transform(BWT)进行了优化，但也要5倍左右内存。FM-index和压缩后缀数组分别针对BWT和后缀数组使用压缩。Succint使用压缩后缀数组方法。

<a href="https://github.com/oeddyo/algorithm/blob/master/resources/%E7%89%9B%E4%BA%BA%E8%B0%88ACM%E7%BB%8F%E9%AA%8C(%E5%8C%85%E6%8B%AC%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F%E8%AE%BA%E6%96%87)/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2009%E8%AE%BA%E6%96%87%E9%9B%86/11.%E7%BD%97%E7%A9%97%E9%AA%9E%E3%80%8A%E5%90%8E%E7%BC%80%E6%95%B0%E7%BB%84%E2%80%94%E2%80%94%E5%A4%84%E7%90%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E6%9C%89%E5%8A%9B%E5%B7%A5%E5%85%B7%E3%80%8B/%E5%90%8E%E7%BC%80%E6%95%B0%E7%BB%84%E2%80%94%E2%80%94%E5%A4%84%E7%90%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E6%9C%89%E5%8A%9B%E5%B7%A5%E5%85%B7.pdf">这里</a>有关于后缀数组的介绍，又想起了原来搞ACM的欢乐时光 LOL

核心思路就是字典序排序生成的$$SA[i]$$，也是论文里面的$$AoS2Input$$，和名次数组$$rank[i]$$，论文里的$$Input2AoS$$。二者互逆。

后面感觉就是将压缩后缀数组应用到query，进行调整优化的问题。同时和MongoDB、Cassandra进行比较。

