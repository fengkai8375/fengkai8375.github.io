---
layout: post
title: "Golang学习笔记之Golang基础"
date: 2018-01-02 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang学习笔记之Golang基础.

### 短声明变量

在函数中，`:=` 简洁赋值语句在明确类型的地方，可以用于替代 var 定义。

函数外的每个语句都必须以关键字开始（`var`、`func`等等），`:=` 结构不能使用在函数外。

```go
func main(){
    a := "Hello"
    b, c, d := 1, "World", false
}
```


### 类型转换

用类型+括号实现类型转换。

```go
var k = float64(2) //  k := float64(2)
```

<!-- more -->

### 常量

用 `const`声明常量，常量不能用`:=`定义。

```go
const  Pi = 3.1415926
```

### 数组

数组声明的方式为`[长度]类型{逗号分隔的元素列表}`，示例如下：

```go
nums := [10]int{}
for i:=0;i<10;i++{
	students[i] = i
}

names := []string["Jim","Tom","John"]

students := []string{}  //长度不定的空数组
```

数组长度通过`len`方法获取
```go
count := len(names)
```

数组切片

```go
names2 := names[:] // 初始化，等同于names的引用
names2 := names[startIndex:endIndex] //从startIndex到endIndex-1中间的元素
names2 := names[startIndex:] //从startIndex到最后
names2 := names[:endIndex] //从开始到endIndex-1
```

### 循环

Golang只有一种循环，`for`，除了不带`()`，其它跟其它语言一样。

```go
func my_loop() int {
	sum := 0
	for i := 0; i < 10; i++{
		sum += i
	}
	return sum
}

func my_loop2() int {
	sum := 1
	for sum < 10000 { //Golang版while
		sum += sum
	}
	return sum
}

func my_loop_array() int {
	names := []string["Jim","Tom","John"]
	for index,name := range names{
	    fmt.Println(name)
	    fmt.Println(index)
	}
}
```

### 条件判断

`if`判断，除了不带`()`，其它也蹑其它语言一样。

### Switch

Golang的`switch`不带`break`

```go
func my_switch(){
	switch os := runtime.GOOS; os{
	case "linux" :
		fmt.Println("linux")
	case "windows" :
		fmt.Println("windows")
	case "osx" :
		fmt.Println("macOS")
	default :
		fmt.Println("default os")
	}
}
```

### defer

`defer`语句会延时执行直到上层函数返回。

延时语句的代码会立刻生成，但直到上层函数返回时它才会被执行。

```go
func main(){
    defer fmt.Println("defer execute")
    fmt.Println("Hello World")
}

```

