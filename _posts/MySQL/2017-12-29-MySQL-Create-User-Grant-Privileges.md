---
layout: post
title: "MySQL新建用户并授权"
date: 2017-12-29 09:00:00 +0800 
categories: MySQL
tag:
 - MySQL
---
* content
{:toc}

### 创建新用户

```shell
use mysql;

# 任意主机，host用 % 
create user 'usename'@'host' IDENTIFIED by 'password';

flush privileges;
```

<!-- more -->

### 创建新的数据库并授权

```shell
CREATE DATABASE IF NOT EXISTS yourdbname DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

# *.* 表示对所有数据库的所有表授权
grant all privileges on database.tablename to 'usename'@'host'; 

flush privileges;
```

