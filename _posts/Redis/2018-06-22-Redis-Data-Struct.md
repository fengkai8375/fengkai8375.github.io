---
layout: post
title: "Redis的数据结构"
date: 2018-06-22 09:00:00 +0800 
categories: Redis
tag:
 - Redis
---
* content
{:toc}

### Redis的数据结构

1. Key/Value
2. Hash
3. List
4. Sets
5. Sorted Sets
6. Geocoding
7. Pub/Sub

<!-- more -->


各有各的使用场景。

KV是简单的键值对；Hash是Hash表，类似数据库里的简单记录；List是双向链表，适合作队列；Sets是集合，值不重复的集合，可以做交集、并集、差集的计算；Sorted Sets是有序集合，是Sets的升级。

Geocoding适合做地理位置信息存储。是新加的特性。