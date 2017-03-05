---
title: Go基础：类型、变量、函数
date: 2017-03-05 21:57:34
tags: [服务器开发]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=35403523&auto=0&height=66"></iframe>

## 运行环境：
    go version go1.8 windows/amd64

## 编辑器
    JetBrains Gogland

## Go 官方test代码

    $GOPATH$\test

## 基本类型
```
    bool
    string
    int  int8  int16  int32  int64
    uint uint8 uint16 uint32 uint64 uintptr
    byte // uint8 的别名
    rune // int32 的别名, 代表一个Unicode码
    float32 float64
    complex64 complex128
```

<!--more-->

## 练习代码

```
    /*包名*/
    package main

    /*导包*/
    import (
        "fmt"
    )

    func main() {
        fmt.Println("Go : Hello World!")
        fmt.Println(add(25, 25))
        fmt.Println(swap("huiyong", "liang"))
        fmt.Println(split(25))
    }


    //1、参数类型相同(x int, y int)，可简写成(x, y int)
    //2、返回类型为 int

    func add(x, y int) int {
        return x + y
    }

    //1、返回类型为 (string, string)，

    func swap(x, y string) (string, string) {
        return y, x
    }


    //1、返回类型(x, y int)，已经声明了返回变量x，y
    //2、指定了返回变量时，return x, y 可简写成 return、
    //3、 `:=` 简洁赋值语句用在明确类型的地方，用于替代 `var`

    //等同
    //func split(sum int) (int, int) {
    //    x := sum * 4 / 9
    //    y := sum - x
    //    return x, y
    //}
    func split(sum int) (x, y int) {
        x = sum * 4 / 9
        y = sum - x
        return
    }
```