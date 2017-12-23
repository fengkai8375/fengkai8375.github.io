---
layout: post
title: "RHEL7启用iptables 停用firewalld"
date: 2017-02-03 09:00:00 +0800 
categories: Linux
tag:
 - firewalld
 - iptables
---
* content
{:toc}

Red Hat Enterprice Linux(RHEL) 7，当然也包括``` CentOS 7 ```，防火墙服务默认使用的是 ``` firewalld ```，而不是 ``` iptables ```。如果想改用 ``` iptables ```，可以参考以下步骤。

## 安装 iptables:

命令

``` shell
yum install -y iptables-services
```

<!-- more -->

## 启用 iptables:

命令: 

``` shell
systemctl mask firewalld
systemctl enable iptables
```

如果需要使用 ip6tables , 需另外加一行

``` shell
systemctl enable ip6tables
```

## 停止firewalld服务，开启 iptables服务

命令: 

``` shell
systemctl stop firewalld
systemctl start iptables
```
同上，如果需要使用 ip6tables , 需另外加一行

``` shell
systemctl start ip6tables
```

然后就可以像以前那样使用 ``` iptable ``` 啦！

