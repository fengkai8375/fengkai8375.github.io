---
layout: post
title: "Golang学习笔记之结构体Struct"
date: 2018-xx-xx 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang学习笔记之结构体Struct.

### 结构Struct声明
```
type HtmlUrl struct{
	url string
	depth int
}
```

### 结构体实例化
```
firstHtml := HtmlUrl{}
firstHtml.url = "http://github.com"
firstHtml.depth = 1
```	

