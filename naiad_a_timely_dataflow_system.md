# Naiad: A Timely Dataflow System

# 1 Introduction

同时支持以下三个功能:stream processor, batch system, trigger-based approach.提出了一个新的timely dataflow框架.

# 2 Timely dataflow

timely dataflow是一个基于有向图的计算模型.每一个节点都有状态,接受沿着边传递的信息,其中包含了时间戳.图结构因为包含了环,时间戳就可以反应这种结构.这样产生的模型就支持不同的epoch和迭代同步执行.

## 2.1 Graph structure

