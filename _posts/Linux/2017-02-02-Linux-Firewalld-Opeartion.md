---
layout: post
title: "Linux firewalld常用操作"
date: 2017-02-02 09:00:00 +0800 
categories: Linux
tag: firewalld
---
* content
{:toc}

列出所有受允许的端口
```shell
firewall-cmd --list-ports
```

添加允许的端口
```shell
firewall-cmd --permanent --add-port=1000-2000/tcp
```

重新载入firewalld
```shell
firewall-cmd --reload
```

