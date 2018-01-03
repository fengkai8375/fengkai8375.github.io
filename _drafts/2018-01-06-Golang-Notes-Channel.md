---
layout: post
title: "Golang学习笔记之channel"
date: 2018-01-06 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang学习笔记之channel

### 声明

channel声明方式，通过`make`方法创建

```go
my_chans := make(chan int, 20) //类型为int，长度为20
```

### channel操作

```go
a := 1
b := 2
my_chans := make(chan int, 20)
my_chans <- a //放入channel
my_chans <- b //放入channel

c := <- mychans  //从channel中取出
d := <- mychans //从channel中取出
```

<!-- more -->

### 超时定时器

防止读取`channel`超时的定时器，超时时间设置为5s。
```go
select {
	case <- time.After(time.Second * 5) :
		fmt.Println("timeout")
	case result := <- results :
		fmt.Println("result:", result)
}
```

同样也可以写个防止写入`channel`超时的定时器。
