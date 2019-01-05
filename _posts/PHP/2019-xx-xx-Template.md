---
layout: post
title: "ThinkPHP5关闭日志写入"
date: 2019-01-05 09:00:00 +0800 
categories: PHP
tag:
 - PHP
 - ThinkPHP
---
* content
{:toc}

ThinkPHP5默认开启了日志的写入。

如果要彻底关闭日志，打开 `application` 目录下的 `config.php`。

找到 `log`那一块儿，可以看到 
``` php
`type` => 'file'
```

`type`可选`file` `socket` `test`，其中`test`为关闭日志写入。 

