---
layout: post
title: "CentOS7最小安装后ifconfig不能用"
date: 2018-01-27 09:00:00 +0800 
categories: Linux
tag:
 - Linux
---
* content
{:toc}

CentOS7执行最小安装后，`ifconfig`命令不能用，提示`command not found`。

网上很多教程，大部分都在说网卡默认没被激活啥的，完全是误人子弟，其实跟网卡没关系，关键在于`net-tools`包没被安装，而`ifconfig`就包含在这个包里，所以只要把`net-tools`装上就好了。

<!-- more -->

```shell
//root执行
yum install net-tools
```

另附一小技巧，使用`yum provides */command`可查命令包含在哪个包里，如

```shell
yum provides */ifconfig
```

结果如下

```shell
net-tools-2.0-0.22.20131004git.el7.x86_64 : Basic networking tools
源    ：base
匹配来源：
文件名    ：/sbin/ifconfig



net-tools-2.0-0.22.20131004git.el7.x86_64 : Basic networking tools
源    ：@anaconda
匹配来源：
文件名    ：/sbin/ifconfig
```

