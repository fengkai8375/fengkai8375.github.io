---
layout: post
title: "MySQL行锁"
date: 2017-08-31 09:00:00 +0800 
categories: MySQL
tag:
 - MySQL
 - InnoDB
---
* content
{:toc}

MySQL中`select`默认不加锁，但可以通过添加`FOR UPDATE`显式地加排他锁。
```
select * from table_name where id=1 FOR UPDATE;
select id,name,passwd from table_name where id=1 FOR UPDATE;
```

当一个事务通过上面的语句给一行数据加上排他锁后，只有当前进程可以对当前事务进行读写操作。在当前进程释放锁前，其它进程都无法再获得该行数据的任何锁，也不能对该行数据进行读写操作。

行锁锁的整行的数据，跟select的字段无关。

> InnoDB行锁是通过给索引项加锁来实现的，即只有通过索引条件检索数据，InnoDB才使用行级锁，否则将使用表锁！

