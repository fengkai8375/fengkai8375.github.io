---
layout: post
title: "PHP调试BUG追踪方法"
date: 2016-02-04 09:00:00 +0800 
categories: PHP
tag: PHP
---
* content
{:toc}

1. 将调试结果输出到数据库或文本文件中

2. 另建一个测试类和配套的其它文件

3. 借助反射或搜索工具定位函数或类的定义

4. 使用归因理论作对比。该帐号（或其它）是否在该环境下常有BUG？该帐号（或其它）是否在其它环境下也有BUG？其它帐号（或其它）是否也在该环境下有BUG？  
     
     原因： 程序上的问题，配置上的问题

5. 日志分析

6. Chrome浏览器可以使用JS的console.log打印调试，AJAX可通过F12 network点击链接查看返回结果
 
7. Chrome的socklog插件

8. XDebug