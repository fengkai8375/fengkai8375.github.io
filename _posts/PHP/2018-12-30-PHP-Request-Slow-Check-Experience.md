---
layout: post
title: "PHP请求慢排查心得"
date: 2018-12-30 09:00:00 +0800 
categories: PHP
tag:
 - PHP
---
* content
{:toc}

### 自查日志方式

1. 配置Nginx的status和php-fpm的status
2. 配置php-fpm的 request_slowlog_time
3. 根据慢日志查找问题所在


### 第三方监控

`听云`用着还行，但是不太准确，没有自己服务器上的日志靠谱。

