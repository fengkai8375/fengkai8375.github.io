---
layout: post
title: "Linux进程"
date: 2018-06-20 09:00:00 +0800 
categories: Linux
tag:
 - Linux
 - PHP
---
* content
{:toc}

### 进程查找，不存在则启动

该示例是将一个`PHP`脚本常驻内存不间断执行。

新建一个shell脚本，名为`script.sh`

<!-- more -->

```shell
#!/bin/bash
. /etc/profile
ps -fe | grep "php mycron.php" | grep -v grep
if [ $? -ne 0 ]
then
echo "not running,start process....."
cd /var/www/html/default
php mycron.php > /dev/null 2>&1 &
else
echo "process is already running....."
fi
```

同时在定时任务中增加
```shell
* * * * * bash /path/to/script.sh
```

注意点：

1. `nohup`是为了在用户退出时进程仍能运行，即进程不会被`hup`挂起。
2. cron中的任务默认是`nohup`执行的，所以无需`nohup`，但是由于cron中的任务不是在终端中运行，也无关用户状态，所以如果要实现进程常驻后台运行，需要在脚本的开头加上 `. .profile`，即自己的配置文件，如果没有该文件，可直接`. /etc/profile`
3. `&`还是要加上的，以使进程在后台运行。
4. 定时任务中无需加`nohup`或`&`


### 杀进程
```shell
kill -s 9 `ps -aux | grep firefox | awk '{print $2}'`
```