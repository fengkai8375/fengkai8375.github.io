---
layout: post
title: "Golang使用goquery解析Html Dom"
date: 2018-01-10 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang中Html Dom解析可使用第三方库`goquery`实现，就像用`jQuery`操作节点一样方便。

### 安装 goquery
```shell
go get github.com/PuerkitoBio/goquery
```

### 引用

```go
import "github.com/PuerkitoBio/goquery"
```

<!-- more -->

### 创建Document

```go
document,err := goquery.NewDocument(url)
if err != nil {
	panic(err)
}
```

### 获取页面标题和所有链接

```go
//页面标题
document_title := document.Find("title").Eq(0).Text()
fmt.Println("Document Title:", document_title)


//页面内所有链接及其标题
document.Find("a").Each(func(i int, selection *goquery.Selection) {
	link,err := selection.Attr("href")
	fmt.Println(err) //err is bool
	fmt.Println("find url:", link)
	title = selection.Text()
	fmt.Println(title)
})
```

