---
layout: post
title: "PHP FPM慢请求日志收集"
date: 2017-11-15 09:00:00 +0800 
categories: PHP
tag: PHP
---
* content
{:toc}

编辑php-fpm配置文件

```shell
vi /etc/php-fpm.d/www.conf
```

将 `request_slowlog_timeout`前的注释取消，并设置为需要的值，默认单位为秒。

比如，定义请求处理时间在1秒以上的查询为慢查询，则将值设置为1 。 

重启 php-fpm 服务。

慢请求的个数也可以在php-fpm的status页面看到。

> 优化不只是优化代码、数据库，还要优化所用服务器的各个软硬组件。