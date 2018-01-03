---
layout: post
title: "Linux Shell 文件和文件夹操作"
date: 2017-12-12 09:00:00 +0800 
categories: Shell
tag: Shell
---
* content
{:toc}

### 文件夹下的文件和文件夹列表

```shell
folder="/home/fengkai/shells"
des="/home/fengkai/sh2"
for file_a in ${folder}/*
do
file_name=`basename $file_a`
echo $file_name
if [ -f $file_name ];then
cp -f $file_name $des
elif [ -d $file_name ];then
echo "${file_name} is a folder,skipped"
fi
done
```