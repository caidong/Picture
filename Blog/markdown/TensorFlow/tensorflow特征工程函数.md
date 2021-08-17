---
title: tensorFlow 特征工程函数
date: 2018-8-29 10:51:21
categories: 
- tensorflow
tags: 机器学习
---

 方法 | 处理数据类型
---|---
numeric_column(key,shape=(1,),default_value=None,dtype=tf.float32,normalizer_fn=None)| 一维实数数字特征
bucketized_column(source_column,boundaries)| 离散的discretized密集dense数据类型
categorical_column_with_hash_bucket(key,hash_bucket_size,dtype=tf.string)|当category的数量很多，也就无法使用指定category的方法来处理了，哈希分桶
categorical_column_with_identity( key,num_buckets,default_value=None)|连续的continue数字类处理
categorical_column_with_vocabulary_file(key,vocabulary_file,vocabulary_size=None,num_oov_buckets=0,default_value=None,  dtype=tf.string)|当词汇表过大时，使用文件
categorical_column_with_vocabulary_list(key,vocabulary_list,dtype=None,    default_value=-1,num_oov_buckets=0)|输入是字符串或整数格式，在内存中将每个值映射一个整数id
embedding_column(categorical_column,dimension,combiner='mean',initializer=None,ckpt_to_load_from=None,tensor_name_in_ckpt=None,max_norm=None,trainable=True)| 输入数据是稀疏的转换成稠密的
shared_embedding_columns | 从稀疏分类转换到密集分类的列表，生成一个二维的列表，且不同分片权重不同
weighted_categorical_column(categorical_column,weight_feature_key,dtype=tf.float32)| 不同分类的权重不同
linear_model(features,feature_columns,units=1,sparse_combiner='sum',weight_collections=None,trainable=True) | 对所有特征进行线性加权
crossed_column(keys,hash_bucket_size,hash_key=None) | 组合特征，这仅仅适用于sparser特征.产生的依然是sparsor特征.
indicator_column(categorical_column)  | 多热值表示给定的分类列 ，计算出对应位置值的统计个数
make_parse_example_spec |创建解析规范字典
input_layer | 基于给定的特征列返回密集张量作为输入层

处理方法：
分桶
one-hot独热编码
虚拟编码

函数返回一个 Categorical-Column 或一个 Dense-Column 对象，但却不会返回 bucketized_column，后者继承自这两个类：
![image](https://img-blog.csdn.net/20180504110220625?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FtYW8xOTk4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



### 示例代码
```
Represents multi-hot representation of given categorical column.

  Used to wrap any `categorical_column_*` (e.g., to feed to DNN). Use
  `embedding_column` if the inputs are sparse.

  ```python
  name = indicator_column(categorical_column_with_vocabulary_list(
      'name', ['bob', 'george', 'wanda'])
  columns = [name, ...]
  features = tf.parse_example(..., features=make_parse_example_spec(columns))
  dense_tensor = input_layer(features, columns)

  dense_tensor == [[1, 0, 0]]  # If "name" bytes_list is ["bob"]
  dense_tensor == [[1, 0, 1]]  # If "name" bytes_list is ["bob", "wanda"]
  dense_tensor == [[2, 0, 0]]  # If "name" bytes_list is ["bob", "bob"]
  

  Args:
    categorical_column: A `_CategoricalColumn` which is created by
      `categorical_column_with_*` or `crossed_column` functions.

  Returns:
    An `_IndicatorColumn`.
```
