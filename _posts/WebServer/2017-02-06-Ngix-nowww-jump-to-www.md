---
layout: post
title: "Nginx不带www跳转到www"
date: 2017-02-06 09:00:00 +0800 
categories: Linux
tag: Nginx
---
* content
{:toc}

```
if ($host = 'test.cn'){
    rewrite ^/(.*)$ http://www.test.cn/$1 permanent;
}
```

