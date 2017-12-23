---
layout: post
title: "Nginx配置php-fpm status"
date: 2017-02-11 09:00:00 +0800 
categories: Linux
tag: Nginx
---
* content
{:toc}

本文介绍了在nginx配置php-fpm status的方法。

###  步骤1

修改 ```nginx.conf```, 在 ```server```块中添加
``` shell
    location ~ ^/php_fpm_status$ {
         include fastcgi_params;
         fastcgi_pass 127.0.0.1:9000;
         fastcgi_param SCRIPT_FILENAME $fastcgi_script_name;
         access_log off;
     }
```

<!-- more -->

### 步骤2
修改 ```php-fpm.conf```，取消 ```pm.status_path = ``` 那一行的注释，并修改为

``` shell
pm.status_path = /php_fpm_status
```

注意等号后的路径要与```nginx```中的路径保持一致！

### 步骤3
重启nginx和php-fpm

``` shell
service nginx restart
service php-fpm restart
```

最后就可以通过 ```http://yourip/php_fpm_status``` 查看 ```php-fpm```当前的状态。

