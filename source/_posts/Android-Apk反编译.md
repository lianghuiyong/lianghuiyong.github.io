---
title: Android Apk反编译
date: 2017-03-30 11:20:06
tags: [Android]
---

### 1、解apk包

    apktool d -s ***.apk
![](http://o9or3yi8n.bkt.clouddn.com/Android%20%E5%8F%8D%E7%BC%96%E8%AF%91.png.png)

### 2、反编译class文件

    dex2jar.bat D:\apktool\d2japk\classes.dex