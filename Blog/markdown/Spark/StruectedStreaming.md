## 问题与解释



### 压力测试

1. kafka 主机，消费端无法连接

![image-20210415113635770](C:\Users\zhucaidong\AppData\Roaming\Typora\typora-user-images\image-20210415113635770.png)



2. ![image-20210415115032799](C:\Users\zhucaidong\AppData\Roaming\Typora\typora-user-images\image-20210415115032799.png)

3.生产端 time_out 



![image-20210415123458138](C:\Users\zhucaidong\AppData\Roaming\Typora\typora-user-images\image-20210415123458138.png)

![image-20210415134146242](C:\Users\zhucaidong\AppData\Roaming\Typora\typora-user-images\image-20210415134146242.png)


##  StructedStreaming 数据字段
root
 |-- key: binary (nullable = true)
 |-- value: binary (nullable = true)
 |-- topic: string (nullable = true)
 |-- partition: integer (nullable = true)
 |-- offset: long (nullable = true)
 |-- timestamp: timestamp (nullable = true)
 |-- timestampType: integer (nullable = true)
