---
title: 配置pyspark 并在Python文件中导入
date: 2021-07-10 23:12:43
tags:
- Pyspark
- 笔记
categories: 
- Pyspark
---
### 配置pyspark 并在Python文件中导入



1. 配置~/.bashrc 文件，并使用 pyspark启动
```
export PYSPARK_DRIVER_PYTHON="jupyter"
export PYSPARK_DRIVER_PYTHON_OPTS='lab  --allow-root'
```
2. 使用find-spark库
```

pip install findspark

import findspark
findspark.init()
import pyspark
```
3. ~/.bashrc 配置完整pyspark路径 

```
export SPARK_HOME=/home/zhucaidong/spark-2.4.7-bin-hadoop2.7
export PYTHONPATH=/home/zhucaidong/pythonEnv/bin/python:$SPARK_HOME/python/lib/py4j-0.10.7-src.zip:$SPARK_HOME/python
export PYSPARK_DRIVER_PYTHON="jupyter"
export PYSPARK_DRIVER_PYTHON_OPTS='lab  --allow-root'
export PYSPARK_PYTHON=python
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
export PATH=$SPARK_HOME:$PATH:~/.local/bin:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
```