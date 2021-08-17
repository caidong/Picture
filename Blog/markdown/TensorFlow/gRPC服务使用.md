---
title: tensorFlow grpc服务使用
date: 2018-8-29 10:51:21
categories: 
- tensorflow
tags: 机器学习
---
##### 1.  使用 TensorFlow Serving 提供服务
1. 安装 tensorflow-model-server
[官方地址](https://www.tensorflow.org/serving/setup#installing_using_apt_get)


2. 安装  tensorflow-serving-api-python3 
```
pip install tensorflow-serving-api-python3 -i https://pypi.douban.com/simple

```



使用 TensorFlow Serving 提供服务
最后，我们来演示一下如何使用 TensorFlow 的姊妹项目 TensorFlow Serving 来基于 SavedModel 对外提供服务。

安装并启动 TensorFlow ModelServer
TensorFlow 服务端代码是使用 C++ 开发的，因此最便捷的安装方式是通过软件源来获取编译好的二进制包。读者可以根据 官方文档 在 Ubuntu 中配置软件源和安装服务端：

1
$ apt-get install tensorflow-model-server
然后就可以使用以下命令启动服务端了，该命令会加载导出目录中最新的一份模型来提供服务：

```
$ tensorflow_model_server --port=9000 --model_base_path=/root/export
2018-05-14 01:05:12.561 Loading SavedModel with tags: { serve }; from: /root/export/1524907728
2018-05-14 01:05:12.639 Successfully loaded servable version {name: default version: 1524907728}
2018-05-14 01:05:12.641 Running ModelServer at 0.0.0.0:9000 ...
```

使用 SDK 访问远程模型
TensorFlow Serving 是基于 gRPC 和 Protocol Buffers 开发的，因此我们需要安装相应的 SDK 包来发起调用。需要注意的是，官方的 TensorFlow Serving API 目前只提供了 Python 2.7 版本的 SDK，不过社区有人贡献了支持 Python 3.x 的软件包，我们可以用以下命令安装：

```
$ pip3 install tensorflow-seving-api-python3==1.7.0
```

调用过程很容易理解：我们首先创建远程连接，向服务端发送 Example 实例列表，并获取预测结果。完整代码可以在 iris_remote.py 中找到。

```
# 创建 gRPC 连接
channel = implementations.insecure_channel('127.0.0.1', 9000)
stub = prediction_service_pb2.beta_create_PredictionService_stub(channel)

# 获取测试数据集，并转换成 Example 实例。
inputs = pd.DateFrame()
examples = [tf.tain.Example() for index, row in inputs.iterrows()]

# 准备 RPC 请求，指定模型名称。
request = classification_pb2.ClassificationRequest()
request.model_spec.name = 'default'
request.input.example_list.examples.extend(examples)

# 获取结果
response = stub.Classify(request, 10.0)
# result {
#   classifications {
#     classes {
#       label: "0"
#       score: 0.998267650604248
#     }
#     ...
#   }
#   ...
# }
```
