---
title: echo框架使用：获取与配置
date: 2017-03-12 17:12:02
tags: [服务器开发]
---

# 运行环境 1
    Linux   :CentOS 7.0
    Go      :go version go1.8 linux/amd64
    GOPATH  :/root/sdk/go/lib

# 运行环境 2
    Windows10
    Go      :go version go1.8 windows/amd64
    GOPATH  :C:\Go\lib

<!--more-->

# 安装 git
    yum install git

# 获取 echo
    go get -v -u github.com/labstack/echo/

# 获取日志
```
github.com/labstack/echo (download)
github.com/labstack/gommon (download)
github.com/mattn/go-colorable (download)
github.com/mattn/go-isatty (download)
github.com/valyala/fasttemplate (download)
Fetching https://golang.org/x/crypto/acme/autocert?go-get=1
https fetch failed: Get https://golang.org/x/crypto/acme/autocert?go-get=1: dial tcp 216.239.37.1:443: i/o timeout
package golang.org/x/crypto/acme/autocert: unrecognized import path "golang.org/x/crypto/acme/autocert" (https fet
ch: Get https://golang.org/x/crypto/acme/autocert?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
```

>1、下到一半的时候可能会出现错误，这时，编译项目的时候缺少哪个包就去对应的下就好了，guthub上也有一些相对应的包（github源的包都能go get到的）

>go get -u -v github.com/stretchr/testify/assert

>2、项目会出现"golang.org/x/net/context"丢失，使用间接获取，

>go get -u -v github.com/golang/net

>在golang.org文件夹下创建x文件夹，将$GOPATH\src\github.com\golang 下的net复制到$GOPATH\src\golang.org\x目录下 ,然后执行：

>go install -v golang.org/x/net/context

>3、运行时可能会出现找不到包cannot find package "github.com/dgrijalva/jwt-go"

>go get -u -v github.com/dgrijalva/jwt-go

# 再安装一遍echo
    go install -v github.com/labstack/echo/

# echo测试代码

```
package main

import (
	"github.com/labstack/echo"
	"github.com/labstack/echo/middleware"
	"net/http"
)

func main() {
	e := echo.New()
	// Middleware
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	// Route => handler
	e.GET("/", func(c echo.Context) error {
		return c.String(http.StatusOK, "Hello, World!\n")
	})

	// Start server
	e.Logger.Fatal(e.Start(":1323"))
}
```

# echo监控

```
⇛ http server started on [::]:1323
{"time":"2017-03-12T18:40:54.7173369+08:00","id":"","remote_ip":"::1","host":"localhost:1323","method":"GET","uri":"/","status":200, "latency":0,"latency_human":"0s","bytes_in":0,"bytes_out":14}
{"time":"2017-03-12T18:40:54.7754925+08:00","id":"","remote_ip":"::1","host":"localhost:1323","method":"GET","uri":"/favicon.ico","status":404, "latency":0,"latency_human":"0s","bytes_in":0,"bytes_out":23}
```

# 本地访问

![](http://oeqej1j2m.bkt.clouddn.com/hello-echo.JPG)


# CentOS
