---
layout: post
title: "Docker常用操作"
date: 2017-01-06 09:00:00 +0800 
categories: Docker
tag: Docker
---
* content
{:toc}

## 登录

```shell
docker login -u username https://index.docker.io/v1/
```

## 创建容器 
```shell
docker run -it -p  <host_port:contain_port> --name mycentos --restart=always centos:centos6 /bin/bash 
# /bin/bash是自启脚本，可替换成 /etc/rc.d/rc.local，但该文件最后要加上 /bin/bash

# --restart=always 表示随系统自启

# -p 是端口映射，可使用多个 -p 参数
# --network=hadoop 指定网络桥
```

<!-- more -->

## 查看网络

```shell
docker network ls
```

## 创建桥接网络

```shell
docker network create --driver=bridge hadoop
docker network inspect hadoop
```

## 查看容器端口

```shell
docker port yourcontainer
```

## 挂载目录

```shell
docker run -it -v /test:/soft centos /bin/bash

# -v的参数，冒号前面是主机的目录，后面是容器的目录，都要使用绝对路径。可使用多个-v参数
```

挂载宿主机已存在目录后，在容器内对其进行操作，报“Permission denied”。可通过两种方式解决：

关闭selinux
```shell
setenforce 0 # 暂时关闭
# 永久关闭的方法：修改/etc/sysconfig/selinux文件，将SELINUX的值设置 为disabled

```

以特权方式启动容器：指定--privileged参数

```shell
# docker run -it --privileged -v /test:/soft centos /bin/bash
```

## 容器相关操作

```shell
docker ps -a
docker ps -l

docker start/stop/rm <containerid>
docker exec -it <containerid> /bin/bash

docker attach <containerid>  # 临时的，exit时会stop容器

#将正在运行的容器持久化成镜像
docker commit <containerid> <yourimageimagename>

#导出容器
docker export <CONTAINER ID> > /tmp/export.tar

#导入容器
cat /tmp/export.tar | docker import - export:latest

```

## 镜像相关操作

```shell
docker images
docker search <keywords>
docker pull <image>

#导出镜像
docer save <imagename> > /tmp/image.tar
docker rm <imagename>
#导入镜像
docker load < /tmp/save.tar

#使用Dockfile构建镜像
docker build -t <yourimage> -f <Dockfile_path>

#删除临时镜像
docker rmi $(docker images --filter dangling=true -q)

#给镜像加标签
docker tag <yourimage> <namespace>/<iamgename>:<version>

# 上传镜像
docker push <namespace>/<iamgename>
```

## 解决centos7.2镜像中服务不能启动的问题
```shell
docker run --privileged  -ti -e "container=docker"  -v /sys/fs/cgroup:/sys/fs/cgroup  centos  /usr/sbin/init
```