---
title: SparkStreaming 数据存储
date: 2021-08-30 23:12:43
tags:
- SparkStreaming
- Pyspark
categories: 
- Pyspark
---

# Spark streaming 数据存储

### 1. Streaming 结果存储在Spark Driver's 内存中，通过JDBC/ODBC客户端访问

   ​	**优点**：低延迟、近实时

   ​    **缺点**：非持久化、易失

   ​    **限制**：Spark Driver‘s内存最大200G,这200G内存中，能用来缓存表格数据，默认参数

   ​	spark.memory.fraction 和 spark.memory.storageFraction,最大的内存使用量 200GB * 0.6 * 0.5= 60GB,

   ​	**突破限制**，可以使用低延迟、大数据集、高性能(Druid、Apache Kudu、Apache Ignite)的分布式内存数据存储，代替单个JVM 内存，单个内存是不够存储的

   - #### 开启步骤

   ##### 1.运行前，开启Spark Thrift Server

   ```java
   from py4j.java_gateway import java_import
   java_import(spark.sparkContext._gateway.jvm, "")
   spark.sparkContext._gateway.jvm.org.apache.spark.sql.hive.thriftserver \
     .HiveThriftServer2.startWithContext(spark._jwrapped)
   ```

   ##### 2. 结果写入内存中供后续查询使用

   ````scala
   tips.writeStream \
     .outputMode("append") \
     .format("memory") \
     .queryName("tips") \
     .option("truncate", False) \
     .start() \
     .awaitTermination()
   ````

   

   ##### 3. 使用JDBC客户端查询

   ```
   /usr/hdp/current/spark2-thriftserver/bin/beeline \
     -u jdbc:hive2://master02.cluster:10016
   ```

   

### 2. 持久化存储到HDFS文件系统

 一旦数据被处理和Spark停止，内存存储是易失和不可靠的。原始数据必须被存储在存储区，一旦数据丢失我们可以从原始数据重建它，我们可以使用多种持久化存储方式，如：关系型数据库、NOSQL存储、对象云存储（S3、Google Cloud）、分布式文件系统（Ceph、GlsterFs、HDFS）

##### 1.数据持久化HDFS 

```python
from pyspark.sql.functions import year, month, dayofmonth

sdfRides.withColumn("year", year("startTime")) \
  .withColumn("month", month("startTime")) \
  .withColumn("day", dayofmonth("startTime")) \
  .writeStream \
  .queryName("Persist the raw data of Taxi Rides") \
  .outputMode("append") \
  .format("parquet") \
  .option("path", "hdfs//namenode:namenode-port/tmp/datalake/RidesRaw") \
  .option("checkpointLocation", "hdfs//namenode:namenode-port/tmp/checkpoints/RidesRaw") \
  .partitionBy("year", "month", "day") \
  .option("truncate", False) \
  .start()

sdfFares.withColumn("year", year("startTime")) \
  .withColumn("month", month("startTime")) \
  .withColumn("day", dayofmonth("startTime")) \
  .writeStream \
  .queryName("Persist the raw data of Taxi Fares") \
  .outputMode("append") \
  .format("parquet") \
  .option("path", "hdfs//namenode:namenode-port/tmp/datalake/FaresRaw") \
  .option("checkpointLocation", "hdfs//namenode:namenode-port/tmp/checkpoints/FaresRaw") \
  .partitionBy("year", "month", "day") \
  .option("truncate", False) \
  .start()
```

##### 2.查询使用结果数据

```python
tips = sdf \
  .groupBy(
    window("endTime", "30 minutes", "10 minutes"),
    "stopNbhd") \
  .agg(avg("tip"))
```

##### 参考文章

[spark-structured-streaming-in-hadoop](https://www.adaltas.com/en/2019/05/28/spark-structured-streaming-in-hadoop/)

