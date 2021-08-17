---
title: RabbitMQ安装
date: 2017-02-24 23:12:43
tags: 
- 笔记
- RabbitMQ
categories: 
- RabbitMQ
---
## 1. 安装Erlang


依赖组件|没装无法使用的apps	|解决
---- | ---- |----
OpenSSL|crypto、ssh、ssl | yum install openssl-devel
Java compiler |	jinterface | yum install java-devel
ODBC library	| odbc |yum install unixODBC-devel
C++ compiler | orber | yum install gcc-c++
    1. 安装构建依赖：

```
$ sudo yum install -y which wget perl openssl-devel make automake autoconf ncurses-devel gcc
```

    2. 从 Erlang 官网下载源代码
```
$ curl -O http://erlang.org/download/otp_src_20.2.tar.gz
```

    3. 解压 tar.gz 包

```
$ tar zxvf otp_src_20.2.tar.gz
```
    4. 安装
```
$ cd otp_src_20.2
$ ./otp_build autoconf
$ ./configure && make && sudo make install
```

    5. 验证


```
$ erl
```


## 2. 安装RabbitMQ 

# 2.1. 安装依赖socat 

```
sudo yum install -y socat
```


elr 已验证可以进入.但是安装rabbitMQ时还是提示依赖Erlang ， 直接使用 --nodeps （不验证依赖）

```
sudo rpm -Uvh rabbitmq-server-3.7.3-1.el7.noarch.rpm  --nodeps
```
**注意** ： rabbitMQ 依赖 Erlang 大于 19.3


2.2 启动服务


```
#启动服务
sudo systemctl start rabbitmq-server

#查看状态
sudo systemctl status rabbitmq-server

#设置为开机启动

systemctl enable rabbitmq-server.service
```
2.3 开启Web管理界面

- 启动管理服务
```
rabbitmq-plugins enable rabbitmq_management
```
- 开放15672端口

#开放端口
sudo firewall-cmd --add-port=15672/tcp --permanent

#重新加载防火墙配置
sudo firewall-cmd --reload
- 配置远程访问

```
#修改配置文件
sudo vi /etc/rabbitmq/rabbitmq.config 

#保存以下内容
[
{rabbit, [{tcp_listeners, [5672]}, {loopback_users, ["admin"]}]}
].
```
- 添加用户

```
#添加用户
sudo rabbitmqctl add_user admin pwd

#设置用户角色
sudo rabbitmqctl set_user_tags admin administrator

#tag（administrator，monitoring，policymaker，management）

#设置用户权限(接受来自所有Host的所有操作)
sudo rabbitmqctl  set_permissions -p "/" admin '.*' '.*' '.*'  

#查看用户权限
sudo rabbitmqctl list_user_permissions admin
```



- RabbitMQ常用命令
```
# 添加用户
sudo rabbitmqctl add_user <username> <password>  

# 删除用户
sudo rabbitmqctl delete_user <username>  

# 修改用户密码
sudo rabbitmqctl change_password <username> <newpassword>  

# 清除用户密码（该用户将不能使用密码登陆，但是可以通过SASL登陆如果配置了SASL认证）
sudo rabbitmqctl clear_password <username> 

# 设置用户tags（相当于角色，包含administrator，monitoring，policymaker，management）
sudo rabbitmqctl set_user_tags <username> <tag>

# 列出所有用户
sudo rabbitmqctl list_users  

# 创建一个vhosts
sudo rabbitmqctl add_vhost <vhostpath>  

# 删除一个vhosts
sudo rabbitmqctl delete_vhost <vhostpath>  

# 列出vhosts
sudo rabbitmqctl list_vhosts [<vhostinfoitem> ...]  

# 针对一个vhosts给用户赋予相关权限；
sudo rabbitmqctl set_permissions [-p <vhostpath>] <user> <conf> <write> <read>  

# 清除一个用户对vhosts的权限；
sudo rabbitmqctl clear_permissions [-p <vhostpath>] <username>  

# 列出哪些用户可以访问该vhosts；
sudo rabbitmqctl list_permissions [-p <vhostpath>]   

# 列出用户访问权限；
sudo rabbitmqctl list_user_permissions <username>

```
- 应用管理
```
rabbitmqctl status //显示RabbitMQ中间件的所有信息
rabbitmqctl stop //停止RabbitMQ应用，关闭节点
rabbitmqctl stop_app //停止RabbitMQ应用
rabbitmqctl start_app //启动RabbitMQ应用
rabbitmqctl restart //重置RabbitMQ节点
rabbitmqctl force_restart //强制重置RabbitMQ节点
```
- 用户管理
```
rabbitmqctl add_user username password //添加用户
rabbitmqctl delete_user username //删除用户
rabbitmqctl change_password username newpassword //修改密码
rabbitmqctl list_users //列出所有用户
```
- 权限控制管理
```
 rabbitmqctl add_vhost vhostpath //创建虚拟主机
 rabbitmqctl delete_vhost vhostpath //删除虚拟主机
 rabbitmqctl list_vhosts //列出所有虚拟主机
 rabbitmqctl set_permissions [-p vhostpath] username <conf> <write> <read> //设置用户权限
 rabbitmqctl clear_permissions [-p vhostpath] username //删除用户权限
 rabbitmqctl list_permissions [-p vhostpath] //列出虚拟机上的所有权限
 rabbitmqctl list_user_permissions username //列出用户权限
 ```
- 集群管理
```
rabbitmqctl cluster_status //获得集群配置信息
rabbitmqctl join_cluster rabbit@localhost --ram | --disc //加入到rabbit节点中，使用内存模式或者磁盘模式
rabbitmqctl change_cluster_node_type disc | ram //修改存储模式
rabbitmqctl set_cluster_name newname //修改名字
```
- 查看管理
```
 rabbitmqctl list_queues [-p <vhostpath>]  //查看所有队列
 rabbitmqctl list_exchanges [-p <vhostpath>] //查看所有交换机
 rabbitmqctl list_bindings [-p <vhostpath>] //查看所有绑定
 rabbitmqctl list_connections //查看所有连接
 rabbitmqctl list_channels //查看所有信道
 rabbitmqctl list_consumers //查看所有消费者信息
```
