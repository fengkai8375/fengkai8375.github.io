---
layout: post
title: "CentOS7安装nginx php7 MySQL"
date: 2017-09-21 09:00:00 +0800 
categories: Linux
tag:
 - PHP
 - MySQL
 - Nginx
---
* content
{:toc}


注意点

1. 先安装memcached
2. 需要安装gcc、zlib、zlib-devel
3. memcache扩展编辑安装完成后需要在`/etc/php.d`手工创建 `memcache.ini`
4. memcache目录下运行`./configure`需要修改php-config的路径

### firewalld
```
systemctl stop firewalld
systemctl disable firewalld
```

<!-- more -->

### Nginx

```
rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum -y install nginx
systemctl enable nginx
systemctl start nginx
```

### PHP
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum install php70w php70w-fpm php70w-devel
yum install php70w-mysqlnd php70w-mbstring php70w-mcrypt php70w-bcmath php70w-gd php70w-xml php70w-xmlrpc
systemctl enable php-fpm
systemctl start php-fpm
```

### MySQL
```
rpm -ivh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum install mysql-community-server
systemctl start mysqld
systemctl enable mysqld
mysql_secure_installation
```

### Memcached和memcache扩展
```
yum install memcached
systemctl start memcached
systemctl enable memcached

yum install git
yum install gcc
yum install zlib zlib-devel

git clone https://github.com/websupport-sk/pecl-memcache memcache
cd memcache
/bin/phpize
./configure --with-php-config=/bin/php-config
make
make install

# 启用memcache扩展
cd /etc/php.d
cp curl.ini memcache.ini
vi memcache.ini
```

### Redis扩展

```
wget http://pecl.php.net/get/redis-3.1.2.tgz
tar -zxvf redis-3.1.2.tgz
cd redis-3.1.2
phpize
./configure --with-php-config=/bin/php-config
make make install 

# 启用redis扩展
cd /etc/php.d
cp curl.ini redis.ini
vi redis.ini
```

### php-fpm
```
user = nginx
group = nginx
php_value[session.save_handler] = memcache
php_value[session.save_path]    = "tcp://127.0.0.1:11211"
```

### selinux

```
setenforce 0

vi /etc/sysconfig/selinux
```