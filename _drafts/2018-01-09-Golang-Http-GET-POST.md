---
layout: post
title: "Golang http GET POST表单"
date: 2018-01-09 09:00:00 +0800 
categories: Golang
tag:
 - Go
---
* content
{:toc}

Golang中GET/POST表单可使用`http`包方便的实现。

### GET
```
url := "http://www.yourl.com"
resp, err := http.get(url)
```

### POST表单

<!-- more -->

#### 方式1

```
url := "http://www.yourl.com";
data := make(url2.Values) // data := url2.Values{}

data.Set("param1", "this is param1") //data["param1"] = []string{"this is param1"}
data.Set("param2", "this is param2") //data["param2"] = []string{"this is param2"}
resp,err := http.PostForm(url, data)

defer resp.Body.Close()
```

#### 方式2

```
url := "http://www.yourl.com";
data := url2.Values{}

data.Set("param3", "this is param3")
data.Set("param4", "this is param4")

client := &http.Client{}

dataStr := data.Encode()
postDataBytes := []byte(dataStr)
postDataByteReader := bytes.NewReader(postDataBytes)
req,err := http.NewRequest("POST", url ,postDataByteReader )
req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
resp,err := client.Do(req)

defer resp.Body.Close()
```

方式1的代码量比方式2少，用起来较方便。

### 获取GET/POST的结果

```
statusCode := resp.StatusCode

content,err := ioutil.ReadAll(resp.Body)

fmt.println(string(content))

```

