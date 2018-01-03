---
layout: post
title: "Golang版爬虫"
date: 2018-01-04 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

自己用Golang写个小爬虫，作为Golang的练手项目。

### 功能需求

- [x] 页面抓取：内容、状态码
- [x] 页面内容解析：DOM、正则
- [x] 抓取深度控制
- [x] 抓取内容存储：文件、数据库
- [x] 并发处理控制
- [x] User-Agent
- [ ] 代理：设置、频繁更换
- [ ] 表单提交
- [ ] cookie处理：接收、发送


### 性能需求

1. 内存占用低 ==整体资源消耗==
2. 阻塞轻 ==中间件==
3. 网络负载不高  ==读取/生产组件==
4. 数据库负载低  ==写入/消费组件==

<!-- more -->

### 依赖库

1. github.com/PuerkitoBio/goquery
2. github.com/go-sql-driver/mysql

### 实现代码

```go
package Spider

import (
	"strings"
	"time"
	"fmt"
	"net/http"
	"github.com/PuerkitoBio/goquery"
	_ "github.com/go-sql-driver/mysql"
	"database/sql"
)

type Config struct {
	MaxDepth int
	MaxConnections int
	StartUrl string
	FetchOutsideLinks bool
	StoreTable string
	DbConfig string
}

type HtmlPage struct{
	url string
	title string
	content string
	statusCode int
	refer string
}

type HtmlUrl struct{
	url string
	depth int
}

func checkErr(err error) {
	if err != nil {
		panic(err)
	}
}

func Fetch(htmlUrl HtmlUrl ,queueUrls chan HtmlUrl,fetchResults chan HtmlPage, config Config){
	if htmlUrl.depth <= config.MaxDepth {
		resp, err := http.Get(htmlUrl.url)
		if err != nil {
			panic(err)
		}

		document,err := goquery.NewDocumentFromResponse(resp) //该方法调用完成后会执行 resp.Body.Close()

		statusCode := resp.StatusCode

		//抓取结果
		page := HtmlPage{}
		page.url = htmlUrl.url
		documentTitle := document.Find("title").Eq(0).Text()
		page.title = documentTitle
		page.statusCode = statusCode
		page.content = document.Text()
		fetchResults <- page

		db, err := sql.Open("mysql", config.DbConfig)
		checkErr(err)

		stmt, err := db.Prepare("INSERT " + config.StoreTable +" SET url=?,title=?,content=?,statusCode=?,depth=?")
		checkErr(err)

		stmt.Exec(page.url, page.title, page.content, page.statusCode, htmlUrl.depth)

		db.Close()

		//页面链接处理
		if htmlUrl.depth <= config.MaxDepth - 1 {
			document.Find("a").Each(func(i int, selection *goquery.Selection) {
				href,err := selection.Attr("href")
				if err == false{
					//fmt.Println("link invalid")
				}
				if !strings.HasPrefix(href,"#") && !strings.HasPrefix(href,"mailto") && !strings.EqualFold(href,"/") {
					if strings.HasPrefix(href, "/"){
						href = config.StartUrl + href
					}
					//是否抓取外链
					if config.FetchOutsideLinks || strings.Contains(href ,config.StartUrl){
						newHtmlUrl := HtmlUrl{}
						newHtmlUrl.url = href
						newHtmlUrl.depth = htmlUrl.depth + 1
						queueUrls <- newHtmlUrl
					}
				}
			})
		}
	}
}

func SpiderGo(config Config){

	firstHtml := HtmlUrl{}
	firstHtml.url = config.StartUrl
	firstHtml.depth = 1

	queueUrls := make(chan  HtmlUrl, config.MaxConnections)
	fetchResults := make(chan HtmlPage, 10000)

	queueUrls <- firstHtml

	for true{

		select {
		case <- time.After(time.Second * 1) :
			fmt.Println("no url in queue")
		case htmlUrl := <- queueUrls :
			go Fetch(htmlUrl, queueUrls, fetchResults, config)
		}

		select {
		case <- time.After(time.Second * 5) :
			fmt.Println("no fetch result yet")
		case result := <- fetchResults :
			fmt.Println("Fetched:",result.title)
		}
	}
}
```

### 开始抓取

```go
func main(){
    config := Spider.Config{}
	config.MaxDepth = 2
	config.MaxConnections = 5
	config.StartUrl = "http://www.fengkai.info"
	config.StoreTable = "pages5"
	config.FetchOutsideLinks = false
	config.DbConfig = "user:password@tcp(127.0.0.1:3306)/test?charset=utf8"
	Spider.SpiderGo(config)
}
```

目前这个小爬虫程序已在Github上开源，最新代码请访问 [spider-go](https://github.com/fengkai8375/spider-go) 
