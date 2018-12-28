---
layout: post
title: "攻击模式处理方案"
date: 2018-12-28 09:00:00 +0800 
categories: Linux
tag:
 - MySQL
 - PHP
---
* content
{:toc}

### 方案（阿里云）

1. RDS添加只读实例，开启读写分离，平时可预留低配实例备用
2. 数据库配置 wait_timeout和 interactive_timeout为 5~10，以便超时自动释放连接，避免连接数过多的问题
2. WAF CC自定义规则，设置单ip规定时长的访问限制及封禁时长，可设置2~3个阶段
3. WAF精准控制规，主要是黑白名单
4. nginx/php-fpm进程保证足够满足正常请求
5. 检查网站代码，通过php-fpm执行的以及通过Cron在终端中执行的，平时一些小问题在有攻击时会被放大严重影响网站运行。

