---
title: python Gevent
date: 2017-08-15 23:12:43
tags:
- 笔记
categories: 
- Python
---
gevent是一个基于协同程序的Python网络库，它使用greenlet在libev事件循环(EvenLoop 注解)之上提供高级同步API。



Event Loop是一个程序结构，用于等待和发送消息和事件。
![image](http://www.ruanyifeng.com/blogimg/asset/201310/2013102004.png)

在gevent中用到的主要模式是Greenlet, 它是以C扩展模块形式接入Python的轻量级协程。 Greenlet全部运行在主程序操作系统进程的内部，但它们被协作式地调度。
