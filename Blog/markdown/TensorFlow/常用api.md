---
title: tensorFlow 常用api
date: 2018-8-29 20:51:21
categories: 
- tensorflow
tags: 机器学习
---
- tf.one_hot() 返回n维的独热张量
```python
import tensorflow as tf
tf.one_hot(indices, depth, on_value, off_value, axis)
输入值：
    indices:  独热向量的位置索引
    depth:  每个独热向量的维度
    on_value: 独热的值
    off_value: 非独热值
    axis: axis指定第几阶为depth维独热向量，默认为-1,即,指定张量的最后一维为独热向量

返回值:
    独热张量
```
- tf.argmax() 返回某个 tensor 对象在某一维上的其数据最大值所在的索引值，相应的有tf.argmin()
```
tf.argmax(input,axis=None,name=None,dimension=None,output_type=tf.int64)
    
输入值：
    input: 以下数据类型的张量: float32, float64, int32, uint8, int16, int8, complex64, int64, qint8, quint8, qint32, bfloat16, uint16, complex128, half, uint32, uint64.
    axis: A Tensor. Must be one of the following types: int32, int64. int32 or int64, must be in the range [-rank(input), rank(input)). Describes which axis of the input Tensor to reduce across. For vectors, use axis = 0.
    output_type: An optional tf.DType from: tf.int32, tf.int64. Defaults to tf.int64.
    name: 操作名 (optional).

返回值:
    类型为output_type的张量
    
```
