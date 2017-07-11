# Discretized Streams: Fault-Tolerance Streaming Computation at Scale

Matei Zaharia, Tathagata Das, Haoyuan Li, Timothy Hunter, Scott Shenker, Ion Stoica

UC Berkeley

传统的流处理流程，都用了连续的有状态的操作子。有两种处理recovery的方法，replication和upstream backup。但是这两种方法都不能讲straggler处理得很好。

Discretized Streams(D-Streams)就能做到，将计算分作是一系列短的、无状态的、确定的任务。简单的理解，D-Streams就是利用了RDD的这个抽象数据结构的特性，优化了流处理：因为RDD很多时候的recover和straggler handling只要处理部分数据partition。



# Appendix

[Spakr Stream一篇不错的介绍](http://www.raincent.com/content-85-4456-1.html)
