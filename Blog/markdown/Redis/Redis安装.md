---
title: Redis安装
date: 2017-06-16 23:12:43
tags: 
- 笔记
- Redis
categories: 
- Redis
---
## Redis 部署安装

Redis是基于REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

1. 下载安装服务

下载地址：http://redis.io/download，下载最新文档版本。

本教程使用的最新文档版本为 2.8.17，下载并安装：
```
#下载地址 wget http://download.redis.io/releases/redis-2.8.17.tar.gz 
文件已下好位于 /Package/Redis/redis-2.8.24.tar.gz

$ tar xzf redis-2.8.17.tar.gz 
$ cd redis-2.8.17
$ make
```

make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用 于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：
```
$ cd src 
$ ./redis-server
```

2. 启动服务

注意这种方式启动redis 使用的是默认配置。也可以通过启动参数告诉redis使用指定配置

文件使用下面命令启动。
```
$ cd src 
$ ./redis-server redis.conf

```

redis.conf是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

3. 测试服务

启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：

```
$ cd src 
$ ./redis-cli 
redis> set foo bar OK 
redis> get foo "bar"
```

4. 开机自启

- 修改redis.conf，打开后台运行选项：
```
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes
```
- 编写脚本，vim /etc/init.d/redis:
```
# chkconfig: 2345 10 90
# description: Start and Stop redis

PATH=/usr/local/bin:/sbin:/usr/bin:/bin

REDISPORT=6379 #实际环境而定
EXEC=/usr/local/redis/src/redis-server #实际环境而定
REDIS_CLI=/usr/local/redis/src/redis-cli #实际环境而定

PIDFILE=/var/run/redis.pid
CONF="/usr/local/redis/redis.conf" #实际环境而定

case "$1" in
        start)
                if [ -f $PIDFILE ]
                then
                        echo "$PIDFILE exists, process is already running or crashed."
                else
                        echo "Starting Redis server..."
                        $EXEC $CONF
                fi
                if [ "$?"="0" ]
                then
                        echo "Redis is running..."
                fi
                ;;
        stop)
                if [ ! -f $PIDFILE ]
                then
                        echo "$PIDFILE exists, process is not running."
                else
                        PID=$(cat $PIDFILE)
                        echo "Stopping..."
                        $REDIS_CLI -p $REDISPORT SHUTDOWN
                        while [ -x $PIDFILE ]
                        do
                                echo "Waiting for Redis to shutdown..."
                                sleep 1
                        done
                        echo "Redis stopped"
                fi
                ;;
        restart|force-reload)
                ${0} stop
                ${0} start
                ;;
        *)
                echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2
                exit 1
esac
```
- 执行权限：

```
chmod +x /etc/init.d/redis
```
- 开机自启动：

```
# 尝试启动或停止redis
service redis start
service redis stop
# 开启服务自启动
chkconfig redis on
```


