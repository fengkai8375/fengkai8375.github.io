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

MySQL中`select`默认不加锁，但可以显式地给数据加上共享锁或排他锁。

### 共享锁

共享锁又称读锁，当一个事务给行加共享锁后，其它事务可以并发读取该行的数据，但任何事务都不能对该行进行写操作。

加共享锁写法：

``` SQL
select * from table_name where id=1 LOCK IN SHARE MODE;
select id,name,passwd from table_name where id=1 LOCK IN SHARE MODE;
```


### 排他锁

排他锁又称为写锁，当一个事务给一行数据加上排他锁后，只有当前进程可以对该行进行读写操作。在当前进程释放锁前，其它进程都无法再获得该行的任何锁，也不能对该行进行读写操作。

加排他锁写法：

``` SQL
select * from table_name where id=1 FOR UPDATE;
select id,name,passwd from table_name where id=1 FOR UPDATE;
```


行锁锁的是整行的数据，跟select的字段无关。

> InnoDB行锁是通过给索引项加锁来实现的，即只有通过索引条件检索数据，InnoDB才使用行级锁，否则将使用表锁！

