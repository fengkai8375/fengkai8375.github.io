---
layout: post
title: "CentOS7 配置netatalk NAS Time Machine"
date: 2018-11-19 09:00:00 +0800 
categories: Linux
tag:
 - CentOS
 - NAS
---
* content
{:toc}

### 下载安装

在[Fedora镜像站](https://copr-be.cloud.fedoraproject.org/results/arrjay/netatalk/epel-7-x86_64/00164217-netatalk)上下载最新的netatalk rpm包

```
wget https://copr-be.cloud.fedoraproject.org/results/arrjay/netatalk/epel-7-x86_64/00164217-netatalk/netatalk-3.1.8-0.1.4.el7.centos.src.rpm
```

安装netatalk
```
yum install netatalk-3.1.8-0.1.4.el7.centos.x86_64.rpm
```

安装依赖包

```
yum install dconf libevent libtdb tracker avahi-libs avahi avahi-autoipd
```


<!-- more -->

### 配置

编辑`/etc/netatalk/afp.conf`

```
[Global]
log level = default:warn
log file = /var/log/afpd.log
;hosts allow = 192.168.0.0/24
spotlight = yes

[Time Machine]
path = /home/myuser
valid users = myuser
time machine = yes
ea = auto
spotlight = no
```

要保证`myuser`对 `/home/myuser`目录有读写权限


### 开启服务
```
systemctl enable avahi-daemon.socket avahi-daemon.service netatalk
systemctl start avahi-daemon.socket avahi-daemon.service netatalk
```

### macOS连接

在Finder菜单栏中"前往->连接服务器"，输入 `afp://xxx.xxx.xxx.xxx`，x部分为你的服务器的ip地址，按提示输入用户名密码就可以连接上了。

然后打开Time Machine，选择上面的Time Machine服务器作为备份磁盘。备份速度受网速影响，内网的话还是很快的。

