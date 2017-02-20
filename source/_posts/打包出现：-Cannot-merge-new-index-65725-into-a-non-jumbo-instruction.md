---
title: 打包出现： Cannot merge new index 65725 into a non-jumbo instruction!
date: 2017-02-20 10:46:37
tags: [Android]
---

# 1、build.gradle里添加
```
android {
  dexOptions {
      jumboMode true
  }
}
```
<!--more-->