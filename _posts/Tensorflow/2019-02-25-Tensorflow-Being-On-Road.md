---
layout: post
title: "踏上Tensorflow的征程"
date: 2019-02-25 09:00:00 +0800 
categories: Tensorflow
tag:
 - Tensorflow
 - Go
---
* content
{:toc}

开始学习Tensorflow前，首先确定一种自己使用Tensorflow的语言，是Python，C，Java，JS，还是Golang？

确定语言之后，再确定环境，是Linux还是macOS？

如果是Linux，最好是在虚拟机中使用Docker。

macOS环境则无需虚拟机。

个人推荐在Docker容器中开发，部署快且不影响主机环境，切换环境也方便。

## 我的环境

1. 主机：macOS 10.13.3
2. 虚拟机:CentOS 7.6
3. Docker容器：CentOS 7.6
4. Tensorflow： Golang with C库

## 配置步骤

1. 配置Docker容器
2. 配置Golang环境
3. 配置Tensorflow C库
4. 配置Tensorflow Golang版

步骤1省略，步骤2参见[CentOS7安装Golang](https://www.fengkai.info/CentOS7-Install-Golang.html)。

<!-- more -->

### 步骤3：配置Tensorflow C库

下载Tensorflow C库
```
wget https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-linux-x86_64-1.12.0.tar.gz
```
解压安装到`/usr/local`
```
tar -C /usr/local -xzf libtensorflow-cpu-linux-x86_64-1.12.0.tar.gz
```

配置链接器
```
ldconfig /usr/local/lib
```

创建示例程序`hello_tf.c`
```
#include <stdio.h>
#include <tensorflow/c/c_api.h>

int main() {
  printf("Hello from TensorFlow C library version %s\n", TF_Version());
  return 0;
}
```

编译
```
gcc -I/usr/local/include -L/usr/local/lib hello_tf.c -ltensorflow -o hello_tf
```
运行
```
./hello_tf
```

输出`Hello from TensorFlow C library version 1.12.0`，代表配置成功

### 步骤4：配置Tensorflow Golang版


下载Tensorflow Go
```
go get github.com/tensorflow/tensorflow/tensorflow/go
```

验证
```
go test github.com/tensorflow/tensorflow/tensorflow/go
```

创建示例程序 `hello_tf.go`
```
package main

import (
    tf "github.com/tensorflow/tensorflow/tensorflow/go"
    "github.com/tensorflow/tensorflow/tensorflow/go/op"
    "fmt"
)

func main() {
    // Construct a graph with an operation that produces a string constant.
    s := op.NewScope()
    c := op.Const(s, "Hello from TensorFlow version " + tf.Version())
    graph, err := s.Finalize()
    if err != nil {
        panic(err)
    }

    // Execute the graph in a session.
    sess, err := tf.NewSession(graph, nil)
    if err != nil {
        panic(err)
    }
    output, err := sess.Run(nil, []tf.Output{c}, nil)
    if err != nil {
        panic(err)
    }
    fmt.Println(output[0].Value())
}
```
运行示例程序
```
go run hello_tf.go
```

输出
```
2019-02-15 07:35:47.006473: I tensorflow/core/platform/cpu_feature_guard.cc:141] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2
Hello from TensorFlow version 1.12.0
```
代表配置成功

