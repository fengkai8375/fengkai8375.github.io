---
layout: post
title: "PHP SESSION心得"
date: 2017-10-21 09:00:00 +0800 
categories: PHP
tag: session
---
* content
{:toc}

1. 单台服务器时，`session.save_handler`用`files`，不要用`memcache`，不然 `session_start` 会有1%左右的机率出现严重耗时的情况。Redis未测试，应该也没有files快。

2. 多台服务器时，session可以存储在Memcache/Redis/Mysql中

<!-- more -->


3. 并发高时，应降低session回收机率，即配置 `session.gc_divisor`

4. 在PHP环境下，默认的 `session.lazy_write = On`无需禁用。不然也会出现1中的问题。