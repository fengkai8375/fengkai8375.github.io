---
layout: post
title: "CentOS7安装配置MySQL 5.7"
date: 2018-01-01 09:00:00 +0800 
categories: Linux
tag:
 - MySQL
---
* content
{:toc}

> 注意：本教程是在CentOS7下完成的，不适用于CentOS6，不然安装MySQL Server时会出现各种库依赖错误，这种情况下，除了升级至CentOS7，没什么好办法。

### 配置yum源

```
# 下载mysql源安装包
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
# 安装mysql源
yum localinstall mysql57-community-release-el7-10.noarch.rpm

# 检查mysql源是否安装成功

yum repolist enabled | grep "mysql.*-community.*"
```

<!-- more -->

### 安装MySQL 5.7

```
yum install mysql
```

### 初始化

```
mysqld --initialize --user=mysql --datadir=/var/lib/mysql
```

如果提示 

```
[ERROR] --initialize specified but the data directory has files in it. Aborting.
[ERROR] Aborting
```

表示 `/var/lib/mysql`目录有文件存在，把这个目录删除后再重装初始化即可。

```
rm -rf /var/lib/mysql
```

初始化完成后会给`root@localhost`生成一个临时密码，保存在`/var/log/mysqld.log`
中，使用这个密码即可登录MySQL。


### 启动mysqld服务

```
systemctl enable mysqld # 开机自启
systemctl start mysqld
```

### 登录MySQL Server

```
mysql -uroot -p  # 然后输入临时密码

```

登录成功后，通过命令重设root密码

```
set password = password("newpassword");
flush privileges;
```

