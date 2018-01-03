---
layout: post
title: "Nginx配置status"
date: 2017-02-08 09:00:00 +0800 
categories: Linux
tag: Nginx
---
* content
{:toc}

在nginx下配置status，按以下步骤执行。

### 步骤1
首先检查nginx编译时有没有包含```http_stub_status_module```
命令：
```shell
nginx -V 2>&1 | grep -o with-http_stub_status_module
```

<!-- more -->

如果出现
```shell
with-http_stub_status_module
```

说明 ```http_stub_status_module```可直接用，无需再编译了。

### 步骤2
修改 ```nginx.conf```, 在 ```server```块中添加
``` shell
    location ~ ^/nginx_status$ {
        stub_status on;
        access_log off;
    }

```

### 步骤3 
重启 nginx
```shell
service nginx restart
```

然后就可以通过 ```http://yourip/nginx_status``` 查看 ```nginx```当前的状态啦！

