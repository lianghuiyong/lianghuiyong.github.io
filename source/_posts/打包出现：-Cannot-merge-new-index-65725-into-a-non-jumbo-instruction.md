---
title: 打包出现： Cannot merge new index 65725 into a non-jumbo instruction!
date: 2017-02-20 10:46:37
tags: [Android]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=155910&auto=0&height=66"></iframe>

# 1、build.gradle里添加
```
android {
  dexOptions {
      jumboMode true
  }
}
```
<!--more-->