---
layout: post
title: "Linux Shell 字符串操作"
date: 2017-12-11 09:00:00 +0800 
categories: Shell
tag: Shell
---
* content
{:toc}

###  字符串长度和简单截取
```
str="Hello,World!"
strlen=${#str} # 字符串长度
echo $strlen
substr1=${str:0:5} # 从开头截取5位
echo $substr1
substr2=${str:0-6:6} # 从末尾第6位起，截取6位
echo $substr2
```