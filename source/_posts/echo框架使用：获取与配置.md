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

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=25862794&auto=0&height=66"></iframe>

# 安装 git
    yum install git

# 获取 echo
    go get -v -u github.com/labstack/echo/

# 获取日志

> 对于下载失败的库，都可以到github.com/golang/去找！

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

![](http://oeqej1j2m.bkt.clouddn.com/echo_test_post.png)

## api.go
```
package api

import (
	"../data"
	"github.com/labstack/echo"
	"net/http"
)

//noinspection GoUnusedExportedFunction
func PostTest(c echo.Context) error{

	movies := []data.Movie{
		{"金刚狼", "2017", []string{"休·杰克曼", "达芙妮·基恩", "帕特里克·斯图尔特"}},
		{"极限特工", "2017", []string{"范·迪塞尔", "露比·罗丝", "妮娜·杜波夫"}},
		{"功夫瑜伽", "2017", []string{"成龙", "李治廷", "张艺兴"}},
		{"生化危机：终章", "2017", []string{"米拉·乔沃维奇", "伊恩·格雷", "艾丽·拉特"}},
	}

	baseMovie := data.BaseResponse{http.StatusOK, "success", movies}

	return c.JSONPretty(http.StatusOK, baseMovie,"    ")
}
```

## BaseResponse.go
```
package data

//Go 默认是不支持泛型的，定义一个接口结构体，即可当泛型使用
type T interface {

}

type BaseResponse struct {
	Code int
	Msg  string
	Data T
}
```

## Movie.go
```
package data

type Movie struct {
	Title  string
	Year   string
	Actors []string
}
```

## main.go
```
package main

import (
	"github.com/labstack/echo"
	"github.com/labstack/echo/middleware"
	"./api"
)

func main() {
	e := echo.New()

	// Middleware
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	//CORS
	e.Use(middleware.CORSWithConfig(middleware.CORSConfig{
		AllowOrigins: []string{"*"},
		AllowMethods: []string{echo.GET, echo.HEAD, echo.PUT, echo.PATCH, echo.POST, echo.DELETE},
	}))

	//static file serviing
	e.Static("/static", "assets")

	// Routers
	e.POST("/", api.PostTest)
	//e.GET("/users/:id", controllers.ShowUser)
	//e.GET("/users", controllers.AllUsers)
	//e.PUT("/users/:id", controllers.UpdateUser)
	//e.DELETE("/users/:id",controllers.DeleteUser)

	// Server
	e.Start(":1323")
}
```

# echo监控

```
"D:\Program Files (x86)\JetBrains\Gogland\bin\runnerw.exe" C:/Go\bin\go.exe run E:/Code/Go-Home/server/main.go
⇛ http server started on [::]:1323
{"time":"2017-03-13T20:05:50.8909566+08:00","id":"","remote_ip":"::1","host":"localhost:1323","method":"POST","uri":"/","status":200, "latency":0,"latency_human":"0s","bytes_in":0,"bytes_out":973}
```

# 请求测试

![](http://oeqej1j2m.bkt.clouddn.com/echo_test_post2.png)


# CentOS 7 防火墙问题

## 1.测试是否可以访问

    telnet 123.56.4.89 1323

## 2.CentOS 7 关闭防火墙

    firewall-cmd --zone=public --add-port=55555/tcp --permanent
    firewall-cmd --reload

> 命令含义： --zone #作用域 --add-port=80/tcp #添加端口，格式为：端口/通讯协议 --permanent #永久生效，没有此参数重启后失效