---
layout: post
title: "Golang学习笔记之自定义package包"
date: 2018-01-07 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang学习笔记之自定义package包

### 新建一个目录

新建一个目录，目录包同要自定义的package名，示例 `Spider`。

### 在这个目录下自定义的Go文件

Go文件的开头须标记`package`名

```go
package Spider
```

<!-- more -->

**注意**：需要被外部访问的变量、方法、结构体等必须以大写字母开头，不然外部访问不了!例如：

```go
func SpiderGo(){
 ...   
}
```

### 在同一个项目中引入它

```go
import `ProjectName/Spider`
```

### 使用package中的方法

```go
Spider.SpiderGo()
```

