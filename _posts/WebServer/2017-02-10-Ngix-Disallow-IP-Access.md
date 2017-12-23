---
layout: post
title: "Nginx禁止ip访问"
date: 2017-02-10 09:00:00 +0800 
categories: Linux
tag: Nginx
---
* content
{:toc}

**nginx**禁止```ip```访问的办法：


修改 nginx.conf，在 ```http```中加上
``` shell
deny 180.97.106.161;
deny 180.97.106.162;
```

把上面的ip换成你要禁止的ip。

<!-- more -->

也可以封禁ip段
``` shell
deny 180.97.106.0/24;
deny 180.97.0.0/16;
deny 180.0.0.0/8;
```

重启 **nginx**

``` shell
service nginx restart
#  service nginx reload 也可以
```

禁止的ip再访问时，会直接出现 ```nginx 403``` 页面：

![nginx 403](https://img.alicdn.com/imgextra/i3/673239777/TB2h_UjqXXXXXbEXXXXXXXXXXXX_!!673239777.jpg)

