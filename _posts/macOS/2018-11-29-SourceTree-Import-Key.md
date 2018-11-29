---
layout: post
title: "macOS下Source Tree导入私钥"
date: 2018-11-29 09:00:00 +0800 
categories: macOS
tag:
 - git
---
* content
{:toc}

macOS下可以使用Source Tree作为git版本管理工具。

Source Tree支持SSH，在终端中执行以下命令可以导入私钥。

```
export PATH=/usr/bin:$PATH
ssh-add -K /path/to/your/id_rsa
```

> 注意：id_rsa的权限不能太开放，不然导入不成功。