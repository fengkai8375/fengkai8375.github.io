---
layout: post
title: "Golang操作MySQL数据库"
date: 2018-01-05 09:00:00 +0800 
categories: Golang
tag:
 - Go
 - MySQL
---
* content
{:toc}

Golang操作MySQL数据库.

### 首先安装`Go`的`MySql`包

```
go get -u github.com/go-sql-driver/mysql
```

### 引入包

```
import "database/sql"
import _ "github.com/go-sql-driver/mysql"
```

### 数据库连接

```
db, err := sql.Open("mysql", "user:password@tcp(127.0.0.1:3306)/dbname?charset=utf8")
```


<!-- more -->

### 插入

```
stmt, err := db.Prepare("INSERT admin SET username=?,nickname=?,email=?")
checkErr(err)

res, err := stmt.Exec("admin666", "网小管", "admin@admin.com")
checkErr(err)

id, err := res.LastInsertId()
checkErr(err)

fmt.Println(id)
```

### 查询

```
rows,err := db.Query("select id,email,username,nickname,password,moble from   admin")
checkErr(err)

for rows.Next() {
	var uid int
	var email string
	var username string
	var nickname string
	var password string
	var moble string
	err = rows.Scan(&uid, &email, &username, &nickname,  &password, &moble)
	checkErr(err)
	fmt.Println("id:",uid)
	fmt.Println("email:",email)
	fmt.Println("username:",username)
	fmt.Println("nickname:",nickname)
	fmt.Println("password:",password)
	fmt.Println("moble:",moble)
}
```

**注意** `row.Scan()`的参数个数要与`select`字段的个数保持一致！

### 更新

```
stmt,err := db.Prepare("update admin set nickname='haha1' where id=?")
checkErr(err)

result,err := stmt.Exec(1)

affected,err := result.RowsAffected() //受影响的行

fmt.Println(affected)
```

删除数据的写法跟更新一样。

```
db.Prepare("delete from admin where id=?")
```

