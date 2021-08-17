---
title: elasticSearch安装
date: 2017-05-12 23:12:43
tags:
- 笔记
- ElasticSearch
categories: 
- ElasticSearch
---
### ES安装
1. 依赖
                
        需保证已安装jdk环境
2. 安装es

```
tar zxvf elasticsearch-2.4.5.tar.gz #解压
cd elasticsearch-2.4.5
bin/elasticsearch

```
4. 日志和乐驰系统es 分别开启实例

           将文件下conf文件上传到主机


3. 添加自启
 ```
 #启动脚本复制到/etc/init.d/ 下 并添加可执行权限
 
  chmod +x lechielasticsearch
 
 #开启自启
  sudo chkconfig lechielasticsearch on
  
 #查看添加是否成功
  
  chkconfig --list
 
 ```
### 如果是用root启动，需要继续下面步骤
1. 如果是用root账号启动，会报以下错误


```
Exception in thread "main" java.lang.RuntimeException: don't run elasticsearch as root.
at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:93)
at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:144) 
at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:285) 
at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35) Refer to the log for complete error details.
```

这是出于系统安全考虑设置的条件。由于ElasticSearch可以接收用户输入的脚本并且执行，为了系统安全考虑， 
建议创建一个单独的用户用来运行ElasticSearch

2. 创建elsearch用户组及elsearch用户


```
groupadd elsearch

useradd elsearch -g elsearch -p elasticsearch
```

3. 更改elasticsearch文件夹及内部文件的所属用户及组为elsearch:elsearch


```
cd /opt
chown -R elsearch:elsearch  elasticsearch
```

4. 切换到elsearch用户再启动


```
su elsearch cd elasticsearch/bin
./elasticsearch
```

启动后打印信息如下
```
[2018-02-26 10:05:30,216][INFO ][node                     ] [Moonhunter] version[2.4.5], pid[19957], build[c849dd1/2017-04-24T16:18:17Z]
[2018-02-26 10:05:30,216][INFO ][node                     ] [Moonhunter] initializing ...
[2018-02-26 10:05:30,500][INFO ][plugins                  ] [Moonhunter] modules [reindex, lang-expression, lang-groovy], plugins [], sites []
[2018-02-26 10:05:30,546][INFO ][env                      ] [Moonhunter] using [1] data paths, mounts [[/opt (/dev/sda5)]], net usable_space [2.6tb], net total_space [2.6tb], spins? [possibly], types [xfs]
[2018-02-26 10:05:30,546][INFO ][env                      ] [Moonhunter] heap size [989.8mb], compressed ordinary object pointers [true]

```


### 文件在root目录下报错

```
bin/elasticsearch: line 78: cd: bin/..: Not a directory
Error: Could not find or load main class org.elasticsearch.bootstrap.Elasticsearch
```
原因是该账号不是root权限，无法cd到指定目录，多半是你把程序解压在了/root/目录下，随便移动到其他位置即可，我移动到了/opt/下面一切正常。

是生产环境别这么做，安全问题可想而知

```
bin/elasticsearch -Des.insecure.allow.root=true  (强制运行root运行)

```

### 安装插件kopf
es 目录下
```
 bin/plugin install lmenezes/elasticsearch-kopf
```
防火墙开启必要的端口 **9200**


### 配置说明

        # Use a descriptive name for your cluster:
        #
        cluster.name: lewell-elastic-log 集群名相同集群自动发现
        #
        #------------------------------------ Node ------------------------------------
        #
        #Use a descriptive name for the node:
        #
        node.name: elastic4 节点描述
        
        # Set the bind address to a specific IP (IPv4 or IPv6):
        #
        network.host: 0.0.0.0 内网其他主机访问
        #
        # Set a custom port for HTTP:
        #
        单机多实例，运行在不同端口
        http.port: 8200
        transport.tcp.port: 8300
        #
        # For more information, see the documentation at:
        # <http://www.elastic.co/guide/en/elasticsearch/reference/current/modules-network.html>
        #
        # --------------------------------- Discovery ----------------------------------
        #
        # Pass an initial list of hosts to perform discovery when new node is started:
        # The default list of hosts is ["127.0.0.1", "[::1]"]
        #
        discovery.zen.ping.unicast.hosts: ["elastic1:8300","elastic2:8300","elastic3:8300","elastic4:8300","elastic5:8300"]
        #
### 安装插件 delete-by-query/
```
 bin/plugin install lmenezes/elasticsearch-delete-by-query
```

### 开启script 乐驰发布程序需要使用

配置文件末尾添加
```
script.inline: on
script.indexed: on
```


    
