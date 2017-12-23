---
layout: post
title: "vsftp相关配置"
date: 2017-02-01 09:00:00 +0800 
categories: Linux
tag: vsftp
---
* content
{:toc}


### 配置文件
```
/etc/vsftpd/vsftpd.conf
```

### 修改端口
```
listen_port=10001
```

### 禁止用户访问其上层目录

```
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list

touch /etc/vsftpd/chroot_list
```

### 添加用户
```
 /usr/sbin/adduser -d /var/www/html/path -g ftp -s /sbin/nologin username
``` 
