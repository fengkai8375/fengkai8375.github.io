---
layout: post
title: "Linux为新硬盘分区格式化并挂载"
date: 2017-11-21 09:00:00 +0800 
categories: Linux
tag: fdisk
---
* content
{:toc}


### 分区

1. 使用`fdisk -l`命令和`df -hT`命令对比查看哪个硬盘没有使用
2. 假设那个没使用的硬盘为 `/dev/vdb`
3. `fdisk /dev/vdb`，对该硬盘分区
4. 建新分区，输入`n`命令，以下可直接用默认值，即使用整个硬盘。
5. 输入`w`保存更改。
6. 更改文件格式类型，还是 `fdisk /dev/vdb`
7. 默认是第一个分区，输入`t`
8. 可使用`L`查看格式类型
9. 新硬盘一般使用`Linux LVM`类型，代号`8e`
10. 输入完直接`w`


<!-- more -->

### 格式化

```shell
mkfs -t xfs /dev/vdb1
```

其中`xfs`是文件系统格式，RHEL7开始，xfs是默认的文件系统格式。RHEL6默认的格式是`ext4`。

### 挂载

将分区`/dev/vdb1`挂载到`/data`目录下
```shell
mkdir /data
mount /dev/vdb1 /data
```

### 开机自动挂载

`vi /etc/fstab` ，将新分区参照其上的格式添加到最后一行，保存退出即可。

示例：

```shell
/dev/vdb1 /data  xfs defaults 0 0
```
