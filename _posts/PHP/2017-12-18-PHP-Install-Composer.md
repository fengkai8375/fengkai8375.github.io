---
layout: post
title: "PHP安装使用Composer"
date: 2017-12-18 09:00:00 +0800 
categories: PHP
tag: Composer
---
* content
{:toc}

### 安装Composer

```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

### 使用Composer

```
composer global require "fxp/composer-asset-plugin:^1.3.1"
composer create-project yiisoft/yii2-app-basic basic 2.0.12
```