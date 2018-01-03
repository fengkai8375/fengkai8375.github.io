---
layout: post
title: "PHP数组foreach循环两种方式及注意点"
date: 2017-12-25 09:00:00 +0800 
categories: PHP
tag: PHP
---
* content
{:toc}

PHP中，数据的```foreach```循环有两种传递方式，按值传递和按引用传递。

## 1. 按值传递

通过数组索引修改原数组
代码示例：
```php
foreach($goods_list as $key=>$row){
    $goods_list[$key]['url'] = url(......);
    .....
}
```

<!-- more -->

## 2. 按引用传递

代码示例

```php
foreach($goods_list as $key=>&$row){
    $row['url'] = url(......);
    .....
}
```

这种方式操作方便，但存在一个潜在的问题。如果在循环过后又使用了 `$row` 这个变量，由于是按引用传递，数组`$goods_list`最后一个元素会发生变化。所以在循环结束后，要加上一行：

```php
unset($row);
```

## 总结

变量在使用的时候，不仅要注意使用前先```初始化```，使用完后更要注意```销毁```。