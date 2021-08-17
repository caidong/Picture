---
title: tensorFlow 高级api Estimator
date: 2018-8-26 20:40:21
categories: 
- tensorflow
tags: 机器学习
---
1. predict() 函数返回
    - predictions 存储的是下列三个键值对：

        - 1 .class_ids 存储的是类别 ID（0、1 或            2），表示模型对此样本最有可能归属的品种做出的预测。
        - 2 .probabilities 存储的是三个概率
        - 3 .logit 存储的是原始对数值
        
- tf.data.DataSet:
    1. 表示很大的数据集
     一个数据集 能够使用能够像集合元素一样 作为输入管道（嵌套的张量结构） 和 作为数据的逻辑转变
    2. 属性
        - output_class:返回一个类每个组件对应元素的数据集 预期返回tf.Tensor 和tf.SparseTensor 
        Return:一个嵌套的Python结构对应每个组件的元素的数据集

        - output_shapes:
         返回数据集的元素的每个组件形状
         Return:一个嵌套的tf.DType 对象 ，每个组件元素的数据集
         
    3. 方法：
    - \__init__ :初始化方法
    - \__iter__ :迭代
    - apply(transformation_func)
     将数据集应用到转换函数上
    - batch(batch_size):
    将数据集中的连续元素组合成一束元素
    - cache(filename=''):
    缓存数据元素到文件
    - concatenate(dataset):
    连接相同的数据集
    -filter(predocate):
    通过predict函数过滤数据集
    - flat_map(map_func):
    maps 通过 map_func 函数映射 同时使结果变平
    - from_generator(generator,out_types,output_shapes)@staticmethod
    由生成器生成的数据集
    - from_saprese_tensor_slices(sparse_tensor)@staticmethod 已废弃。 用 from_tensor_slices()替代
    - from_tensor_slices(tensors)@staticemethod :
    通过张量的切片返回数据集
    - from_tensor(tensors):
    通过张量创建数据集
    - interleave(map_func,cycle_length,block_length=1):
    map_func处理函数，并交错结果集
    - list_files(file_pattern,shuffle=None):
    匹配的所有文件，创建数据集
    - make_initialazable_iterator(shared_name=None):
    通过枚举数据集元素创建一个迭代器
    - Raises():
    抛出异常
    - make_ont_shot_iterator():
    创建一个用于枚举此数据集的迭代器
    - map(map_func,num_parallel_calls=None)
    通过map_func处理数据集
    - padded_batch(batch_size,padder_shapes,padding_values=None)
    - prefetch(buffer_size)
    创建一个从数据集中预取元素的数据集
    - range(*args)@staticmethod
    创建一个步进分割的数据集
    - repeat(count=None) 重复数据集count次
    - shard(num_shards,index) 创建一个仅仅包含1/num_shard 个分片的数据集
    - shuffle(buffer_size,seed=None, reshuffle_each_iteration=None)
    - skip(count)创建一个跳跃过count个元素的数据集 
    - take(count) 创建一个 最多count个元素的数据集
    - zip(datasets) @staticmethod 
    
