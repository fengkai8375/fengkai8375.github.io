---
layout: post
title: "CentOS Git导入SSH key"
date: 2019-01-11 09:00:00 +0800 
categories: Linux
tag:
 - Git
---
* content
{:toc}

CentOS下导入SSH key
```
ssh-agent bash --login -i
ssh-add -k /path/to/key
```

如果不执行`ssh-agent bash --login -i`会报错：`Could not open a connection to your authentication agent.`

<!-- more -->

导入完成后就可以执行
```
git clone git_repo_path
```

其实不导入老的key也行，完全可以用`ssh-keygen`生成一套，再把公钥添加到阿里云Code、Github等Git服务的公钥中去，因为一个账户下是可以有多个公钥的。



