---
layout: post
title: "解决Virtualbox Linux虚拟机挂载共享目录权限的问题"
date: 2018-11-16 09:00:00 +0800 
categories: Linux
tag:
 - Linux
 - CentOS
 - VirtualBox
---
* content
{:toc}

### 问题背景

用VirtualBox创建了一个CentOS的Linux虚拟机，安装完GuestAddon后，共享了一个目录，命名为www，自动挂载到了`/media/sf_www`目录。

CentOS7中安装了nginx和php-fpm，默认以www用户和www用户组执行，但是对挂载的目录没有读写和执行权限。

### 原因分析

挂载目录`/media/sf_www`的所有者是root，用户组是vboxsf，组内的用户有777权限，且其它用户和组一点权限都没有。

尝试将www用户添加到vboxsf组，还是不行。


<!-- more -->

### 解决尝试1

开机自动挂载到另一目录下
```
mount -t vboxsf www /mnt/www
```

挂载后 `/mnt/www`的用户和组都是root，其它用户和组的权限是`xr`，还是达不到写的需求。

### 最终解决办法

最终的解决办法是开机挂载时指定给www和其所在的组。

通过 `id www`查看www用户的id和当前所在组的id，假定用户id为1000，组id为1001，则在开机脚本`/etc/rc.d/rc.local`添加如下命令：

```
mount -t vboxsf -o uid=1000,gid=1001 www /mnt/www/
```

成功挂载后www用户即有对共享文件夹的777权限。



