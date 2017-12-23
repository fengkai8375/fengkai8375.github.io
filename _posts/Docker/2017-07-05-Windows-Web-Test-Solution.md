---
layout: post
title: "Windows Web测试方案"
date: 2017-07-05 09:00:00 +0800 
categories: Docker
tag: Docker
---
* content
{:toc}

### 工具

1. Vmware Workstation 
2. Docker
3. Xshell
4. Ubuntu/CentOS

<!-- more -->

### 方案

*宿主机操作*

1. Win下安装VM，并创建Ubuntu虚拟机
2. 安装VMtool
3. 共享web程序所在文件夹到Ubuntu
4. Ubuntu虚拟机启用SSH
5. xShell连接到Ubuntu虚拟机

*接下来是操作Ubuntu虚拟机*

6. 安装Docker服务
7. Pull Docker镜像
8. 创建Docker容器，并将共享文件夹映射到容器中
9. 配置nginx，创建MySQL数据库
10. 宿主机访问Ubuntu虚拟机，开始测试。