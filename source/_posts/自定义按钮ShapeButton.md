---
title: 自定义按钮ShapeButton
date: 2017-02-28 19:34:35
tags: [Android 自定义UI]
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=28406526&auto=0&height=66"></iframe>


开发时，为了一个简单的按钮要写一堆的样式代码，我表示我在写假代码。

# 地址
[Github 地址](https://github.com/lianghuiyong/CustomUI "Github 地址")

## 可定制属性

```
    app:stroke :细线宽度
    app:radius :拐角宽度
    app:normalColor :按钮颜色
    app:pressColor  :按钮选中颜色
    app:stroke_normalColor :细线颜色
    app:stroke_pressColor :细线选中后的颜色
```


<!--more-->

![](http://o9or3yi8n.bkt.clouddn.com/CustomUI_shape_button.png)

## 示例图代码
```
    <com.better.customui.widget.ShapeButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="123456"
        android:textColor="#FF44FF"
        app:pressColor="#2F000000"
        app:radius="3dip"
        app:stroke="0.5dip"
        app:stroke_normalColor="#553355"
        app:stroke_pressColor="#8F553355" />

```



```
    <com.better.customui.widget.ShapeButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="123456"
        android:textColor="#FF44FF"
        app:normalColor="#FF3988D2"
        app:pressColor="#9F3988D2"
        app:radius="3dp" />
```