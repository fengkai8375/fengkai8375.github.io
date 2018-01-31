---
layout: post
title: "对交易系统架构的探索"
date: 2018-01-31 09:00:00 +0800 
categories: PHP
tag:
 - PHP
 - MySQL
---
* content
{:toc}


最开始接手时，Apache+PHP+MySQL，虽有3台web服务器，但基本相当于单机环境。

接手之后，首先使用Memcache实现了session共享，后加入OSS，使之成为分布式环境。

项目结束之前的架构是：

1. DDoS
2. WAF
3. SLB负载均衡
4. EC2
5. MySQL读写分离
6. Memcache做共享
7. OSS

<!-- more -->

瓶颈在于三个业务：

1. 挂单数据的频繁查询
2. 成交数据的频繁查询
3. 撮合交易

其中挂单数据的查询是最大的瓶颈，MySQL引入读写分离后，主实例压力骤降。但当时只读实例的配置要远小于主实例，这是不科学的。只读实例的配置应该较高才对，因为读的压力大，只读实例要从主实例同步数据，相当于只读实例的写操作和主实例是一样的，额外还有附加的读压力。

一个新的架构，必须要能解决或者明显改善上述三个瓶颈。

最近的探索和尝试：

1. 使用Redis Sortet Sets，配合websocket展示挂单和成交数据。
2. Go语言
3. MQ/Kfaka消息队列
4. 使用Redis List建立消息队列，使撮合成交由同步转为异步，同时降低数据库读取压力。
5. DRDS，分布式数据库。
6. PolarDB，更快的MySQL。
7. Openresty，nginx+lua。

