---
layout: post
title: "PHP多服务器子域名session共享"
date: 2017-10-18 09:00:00 +0800 
categories: PHP
tag:
 - session
 - memcache
---
* content
{:toc}

## 第一步 配置php memcache扩展

```
wget http://pecl.php.net/get/memcache-2.2.7.tgz

tar -xzf memcache-2.2.7.tgz
cd memcache-2.2.7
/usr/local/php/bin/phpize
./configure -with-php-config=/usr/local/php/bin/php-config
make
make install
```

<!-- more -->

编辑 php.ini，启用memcache扩展

```
extension=memcache.so
```

## 第二步 php.ini session配置

```
session.save_handler = memcache

#将127.0.0.1改为你的memcached服务器ip
session.save_path = "tcp://127.0.0.1:11211"  
```

## 第三步 重启php-fpm

```
systemctl restart php-fpm
```


## 第四步 修改php代码

```
//放在程序的最前面，域名前面不要加 .
ini_set('session.cookie_domain', 'yourdomain.com'); 
```