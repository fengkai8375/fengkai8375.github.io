---
layout: post
title: "高并发进程心得"
date: 2018-0622-xx 09:00:00 +0800 
categories: Linux
tag:
 - PHP
 - Nginx
 - Redis
---
* content
{:toc}

### 服务器配置

1. Web: 4C8G 120GSSD
2. MySQL:2C4G

<!-- more -->

### 高并发进程心得


1. 配置nginx status,php-fpm status,nginx/php-fpm日志上传
2. php-fpm的进程数能准确反映当前的并发数，包括CC攻击
3. nginx进程数足够用
4. php-fpm的子进程不要频繁终结和新建，这样会造成额外的开销和连接丢失
5. start_server，min_spare_server,max_spare_server都要大，避免突如其来的连接导致php-fpm进程的增加赶不上连接的增多
6. 单个php-fpm占用的内存不是网上说的30M，平时是6M左右，这点可以从top和free命令查出。最大时估计能达到15M，所以8G内存服务器的最大php进程估计能达到400~500左右，此时MySQL的配置应至少提升到4C8G。
7. MySQL的压力主要在读R，而UCD的操作相比之下非常少，所以读写分离的作用非常明显，只读实例的配置应大于主实例。
8. 目前Web服务器所用120SSD的最大IOPS是4800，当并发约为20时，session/缓存的请求操作约为2400/S，仅此一项已逼近硬盘的最大IOPS！这也是为什么会偶尔出现`session_start`超时。Redis的KV结构每秒读写可轻松上10万，完美应对高并发请求，所以，Session和缓存都应该使用效率更高的Redis，而不是File或Memcached！根据实测，切到Redis后，php-fpm进程的阻塞大大减少，slowlog大幅降低。
9. 组件使用最新的版本，如PHP/Nginx/MySQL/Redis/CentOS