---
layout: post
title: "Memcached不允许外部访问"
date: 2017-01-10 09:00:00 +0800 
categories: Linux
tag: memcache
---
* content
{:toc}

Memcached默认所有ip都可连接，这是不安全的，可以设置只能从内部访问。

```shell
vi /etc/sysconfig/memcached
```

修改 `OPTIONS` 为
```shell
OPTIONS="-l 127.0.0.1"
```

重启 memcached服务即可。

测试方法

```shell
telnet server_ip port 
```
