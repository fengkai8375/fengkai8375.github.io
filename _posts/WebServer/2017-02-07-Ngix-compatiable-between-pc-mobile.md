---
layout: post
title: "Nginx同时适配PC版和手机移动版"
date: 2017-02-07 09:00:00 +0800 
categories: Linux
tag: Nginx
---
* content
{:toc}

项目有个需求，同时存在PC版和手机移动版的页面，全是静态页面，运行在```nginx```下，想要同时适配PC版和手机移动版。

即同一个```URL```，用PC访问量加载PC版页面，用手机访问时加载手机版页面。

由于PC版和手机版每个页面的文件名都是一样的，各有一套css/js/images，如果在用户访问时判断他是通过PC还是手机访问的，从而指定相应的站点```根目录```，问题不就解决了吗？

<!-- more -->

请看```nginx```配置

```shell
server
{
      listen       80;
      server_name  youdomain.com;
      index index.html index.htm index.php;
      

#    listen 443;
#    ssl on;
#    ssl_certificate  /usr/local/nginx/conf/server.crt;
#    ssl_certificate_key  /usr/local/nginx/conf/server_nopwd.key;
      set $mobile_request 0;
      if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
        set  $mobile_request 1;
      }
      location / {
        root /var/www/html/pc/;
        if ($mobile_request = 1) {
            root /var/www/html/mobile/;
        }
      }
      access_log  logs/access_yourdomain_com.log  main;
      error_log logs/error_yourdomain_com.log;
}

```

最后不要忘了重启 nginx 

```shell
service nginx restart
```

注意得用```restart```， ```reload```不管用。

