---
layout: post
title: "解决plupload在IE9下点击没反应的问题"
date: 2017-01-10 09:00:00 +0800 
categories: JavaScript
tag: plupload
---
* content
{:toc}

plupload是一款不错的JavaScript图片上传插件。

plupload在IE9下点击可能会出现没反应的情况，这个问题是由于plupload首选的是```Html5```的上传方式，IE9对```Html5```的支持不好，而且其它上传方式没有被正确配置造成的。

所以我首选```flash```作为上传图片的方式。配置如下：

```
runtimes : 'flash,html5,silverlight,browserplus,gears,html4',
flash_swf_url : '/javascript/plupload/Moxie.swf',
silverlight_xap_url : '/javascript/plupload/Moxie.xap',
```

问题得到解决。

