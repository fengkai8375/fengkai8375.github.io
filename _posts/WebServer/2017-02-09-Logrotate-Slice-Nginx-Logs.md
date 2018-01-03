---
layout: post
title: "使用logrotate切割Nginx日志"
date: 2017-02-09 09:00:00 +0800 
categories: Linux
tag: 
 - Nginx
 - logrotate
---
* content
{:toc}

大体上有三种方法切割```nginx```日志。

第一种是写个定时任务，每天零点把旧的日志重命名，并对nginx进程发送USR1信号使其重新打开日志并写入。

第二种是能过管道的方式把新产生的日志写到另外一个日志文件里。

第三种是能```过logrotate```来切割日志，```logrotate```是系统自带的服务，可以切割任何日志，不仅仅是```nginx```，这里推荐使用它。


<!-- more -->

## 步骤1

编辑 /etc/logrotate.d/nginx 
```shell
vi /etc/logrotate.d/nginx  
```
内容如下

``` shell
/usr/local/nginx/logs/access.log {   
daily 
rotate 7  
missingok 
notifempty 
dateext 
sharedscripts 
postrotate 
    if [ -f /usr/local/nginx/logs/nginx.pid ]; then 
        kill -USR1 `cat /usr/local/nginx/logs/nginx.pid` 
    fi 
endscript 
} 
```

## 步骤2 测试是否可用
```shell
/usr/sbin/logrotate -f /etc/logrotate.d/nginx 
```

## 步骤3 配置定时任务
```shell
crontab -e
```
然后添加一行：
```shell
00 00 * * *  /usr/sbin/logrotate -f /etc/logrotate.d/nginx
```

