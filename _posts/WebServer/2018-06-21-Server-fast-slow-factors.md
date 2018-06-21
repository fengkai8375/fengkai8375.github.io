---
layout: post
title: "服务器快慢因素"
date: 2018-xx-xx 09:00:00 +0800 
categories: Linux
tag:
 - PHP
 - MySQL
---
* content
{:toc}

### 快慢因素
1. 内存
2. 硬盘
3. CPU
4. OS Kernel
5. Web Server CPU、内存、IOPS、最大连接数等配置
6. PHP/php-fpm
7. MySQL CPU、内存、IOPS、最大连接数等配置
8. pdo连接延迟
9. netstat TIME_WAIT

<!-- more -->

### 指标收集

1. Web:CPU/内存
2. Web:php-fpm:活跃进程数用内存占比
3. Web:php-fpm:慢日志
4. Web:php-fpm:404/502请求
5. Web:php-fpm:队列长度
6. Web:php-fpm:每秒请求 (Aliyun Log)
7. Web:IOPS
8. Web:连接数 (netstat/nginx)
9. MySQL:CPU
10. MySQL：会话/活跃会话
11. MySQL: TPS
12. MySQL: select/s
13. MySQL: innodb_rows_read/s
14. MySQL: 缓存读取次数/s
15. MySQL: 临时表占比
16. MySQL: (某段时间)全量日志及消耗时间明细
17. MySQL: 主从延迟
18. WAF: QPS
19. WAF: 带宽
20. Redis: info stats