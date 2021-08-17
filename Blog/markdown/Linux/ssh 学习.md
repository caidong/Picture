---
title: SSH
date: 2017-02-14 23:12:43
tags:
- Linux
- 笔记
categories: 
- Linux
---
## 1. 概念
ssh:ssh是一种网络协议,用于计算机之间的登录.

## 2.基本用法
- 远程登录

```
ssh 用户名@主机 -p 端口

```
## 3.中间人攻击
- ssh工作方式:

公钥加密, 登录远程主机成功,远程主机将自己的公钥发送给本机,本机后续通信使用远程主机的公钥加密后进行传输.
- 中间人攻击:

在本机和远程主机中存在一个非法主机,本机的请求本非法主机截获,后非法主机将请求转发到远程主机,从而在登录主机和本机间不知情的情况下,截获通信信息的攻击方式,被称为中间人攻击.


## 4. 公钥登录:

用户将自己的公钥上传到远程登录主机,登录时远程主机时,远程主机会向用户发送一段随机字符串,用户用自己的私钥加密后再发回来,远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码
- 生成公钥

```
ssh -keygen
```
运行结束以后，在$HOME/.ssh/目录下，会新生成两个文件：id_rsa.pub和id_rsa。前者是你的公钥，后者是你的私钥。

- 上传公钥
 ssh-copy-id user@host

- 检查远程主机配置

如果还是不行，就打开远程主机的/etc/ssh/sshd_config这个文件，检查下面几行前面"#"注释是否取掉。


```
    RSAAuthentication yes
　　 PubkeyAuthentication yes
　　 AuthorizedKeysFile .ssh/authorized_keys
```

## 4. 端口转发和远程操作

- 绑定本地端口

将本地端口转发到远程主机


```
$ ssh -D 8080 user@host
```
SSH会建立一个socket，去监听本地的8080端口。一旦有数据传向那个端口，就自动把它转移到SSH连接上面，发往远程主机

- ssh反向隧道

- ssh 突破防火墙


## 5 ssh 翻墙
客户端连接远程主机 即可开启本地7001 sock 代理端口
    ssh -D 7001 username@remote-host





参考地址:http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html
