---
layout: post
title: "Linux用户和组操作"
date: 2017-02-04 09:00:00 +0800 
categories: Linux
tag:
---
* content
{:toc}


#### 添加用户

添加用户`ftpuser`到组`ftp`并指定目录 `/opt/ftp`

```
# -s /sbin/nologin 表示不可通过SSH登录
/usr/sbin/adduser -d /opt/ftp -g ftp -s /sbin/nologin ftpuser   
```


#### 删除用户

```
userdel ftpuser #别忘删除用户主目录
```

#### 修改密码

```
passwd ftpuser
```

#### 将用户添加到组

```
usermod -g groupname username
```
