---
layout: post
title: "Golang学习笔记之结构体Struct"
date: 2018-01-03 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang学习笔记之结构体Struct.

### 结构Struct声明
```go
type HtmlUrl struct{
	url string
	depth int
}
```

### 结构体实例化
```go
firstHtml := HtmlUrl{}
firstHtml.url = "http://github.com"
firstHtml.depth = 1
```	

