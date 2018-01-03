---
layout: post
title: "MySQL修改用户密码"
date: 2017-09-08 09:00:00 +0800 
categories: MySQL
tag:
 - MySQL
---
* content
{:toc}

#### 方法1
适用于版本`<=5.6`的MySQL及已经重置过密码的MySQL5.7。
```shell
use mysql; 
update user set password=password('123') where user='root' and host='localhost'; 
flush privileges; 
```

<!-- more -->

### 方法2

该方法适用于刚刚初始化完的MySQL5.7数据库
```shell
set password = password("newpass");
flush privileges; 
```

