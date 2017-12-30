---
layout: post
title: "MySQL 5.6升级到5.7注意点"
date: 2017-12-30 09:00:00 +0800 
categories: MySQL
tag:
 - MySQL
---
* content
{:toc}

**千万记得备份！**

1. 安装方式出现变化，5.7安装完后需要先`初始化`再开启`mysqld`服务，且初始化完成后修改`root`密码的方式变了。

2. 一些变量不再支持，如 `thread_concurrency`，如果你配置了这个变量，删除它，不然不能开启`mysqld`服务。

3. `sql_mode`默认是`NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES`，即严格模式。

