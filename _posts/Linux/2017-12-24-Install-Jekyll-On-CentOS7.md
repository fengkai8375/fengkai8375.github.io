---
layout: post
title: "CentOS7 Jekyll安装与使用"
date: 2017-12-23 09:00:00 +0800 
categories: Linux
tag: Jekyll
---
* content
{:toc}


### 安装Ruby

```
yum install centos-release-scl-rh
yum install rh-ruby23  -y  # jekyll要求ruby版本大于2.2.5
scl  enable  rh-ruby23 bash
ruby -v # 查看ruby版本
gem -v  # 查看gem版本
```

<!-- more -->

### 安装jekyll
```
gem install jekyll
```

### 安装jekyll-paginate
```
gem install jekyll-paginate
```

### 运行jekyll

切换到Github Page仓库所在的目录
```
jekyll serve -w --host=0.0.0.0 --incremental # 外网可访问，增量模式
jekyll build  # 构建应用
```
