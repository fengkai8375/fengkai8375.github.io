---
layout: post
title: "rsync配合inotify实时同步服务器文件"
date: 2017-11-20 09:00:00 +0800 
categories: Linux
tag:
 - rsync
 - inotify
---
* content
{:toc}


### 备份服务器配置

安装配置rsync
```shell
yum install rsync
mkdir /usr/local/rsync
echo "backup:bk_password" > /usr/local/rsync/rsyncd.passwd
cd /usr/local/rsync
chmod 600 rsyncd.passwd
```

<!-- more -->

编辑rsync配置文件
```shell
vi /etc/rsyncd.conf
```

内容为 
```shell
uid = root
gid = root
use chroot = no
max connections = 4
strict modes = yes
hosts allow = 192.168.1.101   #可以多个，用空格分隔
port = 873
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsyncd.log

# web是模块名

[web]
path = /var/www
ignore errors
read only = false
list = false
auth users = backup
secrets file = /usr/local/rsync/rsyncd.passwd
```

开启服务
```shell
systemctl start rsyncd
systemctl enable rsyncd
```

### 主服务器配置

安装配置rsync

```shell
yum install rsync
mkdir /usr/local/rsync
echo "bk_password" > /usr/local/rsync/rsyncd.passwd
cd /usr/local/rsync
chmod 600 rsyncd.passwd
```

编辑rsync配置文件
```shell
vi /etc/rsyncd.conf
```

内容为 
```shell
uid = root
gid = root
use chroot = no
max connections = 4
strict modes = yes
hosts allow = 192.168.1.102   #可以多个，用空格分隔
port = 873
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsyncd.log

# web是模块名，主服务器保留这一块是为了初始的同步。

[web]
path = /var/www
ignore errors
read only = false
list = false
auth users = backup
secrets file = /usr/local/rsync/rsyncd.passwd
```


开启服务
```shell
systemctl start rsyncd
systemctl enable rsyncd
```

同步一次文件

```shell
rsync -vzrtopg --delete --progress --password-file=/usr/local/rsync/rsyncd.passwd backup@192.168.1.102
```

安装配置inofity

```shell
yum install inotify-tools
cd /usr/local/rsync
vi inotify_rsync.sh
```

内容为

```shell
#!/bin/bash
host=192.168.1.102
src=/var/www/
des=web
user=backup

/usr/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w%f%e' -e modify,delete,create,attrib $src | while read files  
do 
/usr/bin/rsync -vzrtopg --progress --delete --password-file=/usr/local/rsync/rsyncd.passwd $src $user@$host::$des 
echo "${files} was rsynced" >>/var/log/rsync.log 2>&1 
done
```

其中 host为备份服务器，src为要同步的目录，des为模块名，user为同步所使用的用户。

`/usr/bin/rsync`的参数中 `--delete`为删除，意指当主服务器删除文件时，备份服务器也跟着删除，没有这个参数时不会删除。

最后，开启inotify的实时同步
```shell
chmod 764 inotify_rsync.sh
# 可加到/etc/rc.d/rc.local开机自启
sh /usr/local/rsync/inotify_rsync.sh & 
```

> 先启动主备上的rsyncd，再启动实时同步脚本。如果有多个目录需要同步，可配置多个模块和sh文件。
