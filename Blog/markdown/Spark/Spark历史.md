---
title: Spark历史
date: 2021-07-10 23:12:43
tags:
- Spark
- 笔记
categories: 
- Spark
---



## 历史:

 诞生于2009年,加州大学伯克利分校RAD实验室的一个研究项目,最初是基于Hadoop Mapreduce的
发现Mapreduce 在迭代 式计算和交互式上的低效,引入了内存存储的技术
2010年3月份开源
2011年在AMP实验室在Spark上开发高级组件,像Spark Streaming
2013 年转移到Apache 下,不久便成了顶级项目



- Spark 组件
1. Spark Core 
    -  包含Spark 的基本功能,包含任务调度,内存管理,容错机制等,内部定义了RDDs(弹性分布式数据集).提供了很多APIs来创建和操作这些RDDs,应用场景,为了其他组件提供底层服务.
    
2. SPark SQL
    - 是Spark 处理结构换数据的库, 就像HiveSQL Mysql类似,应用场景,企业中用来做报表统计.
3. Spark Streaming 
    - 实时数据流处理组件,类似Strom
    - Spark Streaming 提供了API来操作实时流数据,应用场景,在企业中用来做从Kafka来接受数据做实时统计.
4. Mlib 
    - 一个包含通用机器学习功能的包,Machine learning lib 
    - 包含分类,聚类 回归等,还包括模型评估和数据导入.
    - MLLib 提供的方法,都支持集群上的横向扩展.
    应用场景机器学习
5. Graphx 
    - 处理图的库 例如社交网络图 , 并进行图的并行计算. 像Spark Streaming ,Spark SQL一样,它继承了 RDD API 
    - 提供了各种图的操作, 和常用的图算法,例如PangeRank算法.
    应用场景图计算
6. Cluster Managers:
    - 集群管理 Spark 自带的一个集群管理是单独调度器.常见集群管理包括 Hadoop YARN ,Apache Mesos 
7. 紧密集成的优点;
    - Spark 底层优化了,基于Spark 底层优化的组件,也得到了相应的优化
    - 紧密集成,节省了各个组件组合使用部署,测试等时间.
    -  向SPARK 增加新的组件时其他组件,可以立即享用新组件的功能