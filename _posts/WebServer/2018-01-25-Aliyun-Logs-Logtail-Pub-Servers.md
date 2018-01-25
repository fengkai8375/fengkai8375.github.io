---
layout: post
title: "阿里云日志服务Logtail公网服务器"
date: 2018-01-25 09:00:00 +0800 
categories: Linux
tag:
 - Logtail
---
* content
{:toc}

这篇文章针对的是非阿里云ECS的公网服务器使用日志服务Logtail的指南。

### 安装Logtail
```shell
wget http://logtail-release.oss-cn-hangzhou.aliyuncs.com/linux64/logtail.sh; chmod 755 logtail.sh; sh logtail.sh install cn_beijing_internet
```

Logtail被安装到`/user/local/ilogtail`

### 配置阿里云账号ID
```shell
touch /etc/ilogtail/users/阿里云账号ID
```

<!-- more -->

### 配置userdeined-id
```shell
vi /etc/ilogtail/user_defined_id
# 填上用户自定义的机器标识
```

### 生成accessKey

这一步一定要做，不然Logtail无权限上传日志。注意：是生成accessKeys，而不是新版的子账号的访问控制。

### 重启Logtail

```shell
/etc/init.d/logtaild stop
/etc/init.d/logtaild start
```

### 过滤静态资源

logtail配置中添加过滤器
```
key: request_uri
Regx: ^(?!.*?(js|css|png|jpg|jpeg|ico|woff)).*
```

