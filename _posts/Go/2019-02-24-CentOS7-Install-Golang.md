---
layout: post
title: "CentOS7安装Golang"
date: 2019-02-24 09:00:00 +0800 
categories: Golang
tag:
 - CentOS
 - Go
---
* content
{:toc}

CentOS 7安装Golang

### 下载安装
```
wget https://dl.google.com/go/go1.10.8.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.10.8.linux-amd64.tar.gz
```

### 配置环境变量
`vi /etc/profile`，在文件末尾加入一行
```
export PATH=$PATH:/usr/local/go/bin
```

### 使环境变量生效
```
source /etc/profile
```

###  测试
```
go
```

