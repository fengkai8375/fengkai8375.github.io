---
layout: post
title: "Redis的主从安装"
date: 2018-01-29 09:00:00 +0800 
categories: Redis
tag:
 - Redis
---
* content
{:toc}

### Redis安装

```shell
#获取redis软件包
#在redis的官方网站（http://www.redis.io）下载最新的稳定版本
wget http://redis.googlecode.com/files/redis-3.1.2.tar.gz
tar -zxvf redis-3.1.2.tar.gz
cd redis-3.1.2
make
make install
```

<!-- more -->

### 配置

```shell
#创建配置目录和配置
mkdir -p /etc/redis
cp redis.conf /etc/redis
vi /etc/redis/redis.conf 
daemonize yes  #redis 以后台进程运行
pidfile /var/run/redis.pid  #给后台进程绑定一个pid
loglevel notice
logfile /data/logs/redis/redis.log
bind 192.168.10.77
appendonly yes

#在从上面配置主，主上面可以不动
slaveof 192.168.10.77 6379
```

### 启动redis

```shell
/usr/local/bin/redis-server /etc/redis/redis.conf &
```


### 检查安装

```shell
/usr/local/bin/redis-cli -h 192.168.10.77
set name admin
OK
get name
"admin"
```

### 检查从是否同步

```shell
/usr/local/bin/redis-cli -h 192.168.10.78
get name
"admin"
#成功
```

###  开启AOF

```shell
vi redis.conf
appendonly yes  #需同时修改appendonly文件名
```

> 新版的RHEL7/CentOS7中可以直接通过 yum install redis安装Redis，配置文件位置 /etc/redis.conf 。

