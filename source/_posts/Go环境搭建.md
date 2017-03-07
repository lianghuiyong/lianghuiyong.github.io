---
title: Go环境搭建
date: 2017-03-01 21:51:01
tags: [服务器开发]
---

## 1、将go sdk解压到 /root/sdk文件夹

```
    # tar -xzf go1.7.linux-amd64.tar.gz -C /root/sdk
```

## 2、.bashrc 添加path
```
    export GOROOT=/root/sdk/go
    export GOPATH=$GOROOT/lib
    export PATH=$PATH:$GOROOT/bin
```

<!--more-->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=2001320&auto=0&height=66"></iframe>

## 3、使path生效
```
   # source .bashrc
```

## 4、go env 查看信息
![](http://oeqej1j2m.bkt.clouddn.com/go_sdk_add_to_linux_path.JPG)
