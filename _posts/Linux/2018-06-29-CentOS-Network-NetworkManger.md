---
layout: post
title: "记一次CentOS7中网卡不能上网的问题解决"
date: 2018-06-29 09:00:00 +0800 
categories: Linux
tag:
 - Linux
---
* content
{:toc}

### 环境

运行于Vmware WorkStation 12下的CentOS 7.4 

### 问题背景

一次`yum update`后重启无论如何上不上网。ifconfig只能看到两个网卡 `lo`和`virbr0`，原来应该有的`eth0`没有了。

<!-- more -->

### 解决之路

1. 首先重启，不管用。

2. 在VMware中断开网卡连接并重新连接，不管用。

3. 网卡连接模式由桥接改为NAT，不管用。

4. 按照网上的思路重新编辑 `/etc/sysconfig/network-scripts/ifcfg-ens33`的内容，并执行 
    ```shell
    ifconfig ens33 down 
    ifconfig ens33 up
    systemctl restart network
    systemctl restart NetworkManger
    ```
    还是不管用。
    
又查阅了一些资料，说是 `NetworkManger`和`network`服务有冲突，不能同时运行。停止并禁用`NetworkManger`，但重新执行`systemctl restart network`后正常了。

> `NetworkManger`和`network`服务不能同时存在，只能保留一个。