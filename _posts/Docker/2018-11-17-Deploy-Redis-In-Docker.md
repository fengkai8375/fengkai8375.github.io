---
layout: post
title: "Redis Docker部署"
date: 2018-11-17 09:00:00 +0800 
categories: Docker
tag:
 - Docker
 - Redis
---
* content
{:toc}


### 下载Redis Docker Image

```
docker pull redis
```

### 创建容器

```
docker run -v /www/server/redis:/usr/local/etc/redis/redis.conf -p 16379:6379 --restart=always  --name redis1 redis redis-server /usr/local/etc/redis/redis.conf &
```

此种方式是挂载了本地的Redis配置文件到容器中并指定为Redis的配置文件。

