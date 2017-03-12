---
title: GO基础：复合数据类型
date: 2017-03-12 16:14:43
tags: [服务器开发]
---

## 运行环境：
    go version go1.8 windows/amd64

## 编辑器
    JetBrains Gogland

## Go 官方test代码

    $GOPATH$\test

<!--more-->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=5307435&auto=0&height=66"></iframe>

## 练习代码

```
package main

import (
	"fmt"
	"encoding/json"
	"log"
)

func main() {
	fmt.Println("--------------数组测试---------------")
	test_array()
	fmt.Println("--------------切片测试---------------")
	test_slice()
	fmt.Println("--------------map测试---------------")
	test_map()
	fmt.Println("--------------json测试---------------")
	test_json()
}

/*-----测试数组-----*/
func test_array() {
	arr := []int{1, 2, 3, 4}
	fmt.Println(arr)
	arr[0] = 3
	arr[1] = 3
	arr[2] = 3
	arr[3] = 3
	fmt.Println(arr)
}

/*------测试切片-----*/
func test_slice() {
	mouth := []string{1: "一月", 2: "二月", 3: "三月", 4: "四月", 5: "五月", 6: "六月", 7: "七月", 8: "八月", 9: "九月", 10: "十月", 11: "十一月", 12: "十二月"}

	slice1 := mouth[1:3]
	slice2 := mouth[2:5]

	fmt.Println(slice1)
	fmt.Println(slice2)

	fmt.Println(mouth)
	reverse(mouth[:])
	fmt.Println(mouth)

	fmt.Println(slice1)
	fmt.Println(slice2)

}

/*--------数组反转--------*/
func reverse(s []string) {
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		s[i], s[j] = s[j], s[i]
	}
}

func test_map() {
	//创建map1
	map1 := map[int]string{}

	//初始化赋值
	map1[1] = "hello1"
	map1[2] = "world2"
	map1[3] = "world3"
	map1[4] = "world4"
	map1[5] = "world5"
	map1[6] = "world6"
	map1[7] = "world7"
	map1[8] = "world8"
	map1[9] = "world9"
	map1[10] = "!!!!!!10"

	//取值
	fmt.Println(map1)
	fmt.Println(map1[1])

	// Map的迭代顺序是不确定的，并且不同的哈希函数实现可能导致不同的遍历顺序。
	// 在实践中，遍历的顺序是随机的，每一次遍历的顺序都不相同。这是故意的，
	// 每次都使用随机的遍历顺序可以强制要求程序不会依赖具体的哈希函数实现。
	//for _, value := range map1 {
	//	println("\t", value, "\n")
	//}

	//删除
	delete(map1, 2)
	fmt.Println(map1)
	fmt.Println(map1[4] + "1")
}

//使用 json解析的结构体成员变量首字母必须大写！！！
func test_json() {

	//Go 默认是不支持泛型的，定义一个接口结构体，即可当泛型使用
	type T interface {
	}

	type Movie struct {
		Title  string
		Year   string
		Actors []string
	}

	type BaseResponse struct {
		Code int
		Msg  string
		Data T
	}

	movies := []Movie{
		{"金刚狼", "2017", []string{"休·杰克曼", "达芙妮·基恩", "帕特里克·斯图尔特"}},
		{"极限特工", "2017", []string{"范·迪塞尔", "露比·罗丝", "妮娜·杜波夫"}},
		{"功夫瑜伽", "2017", []string{"成龙", "李治廷", "张艺兴"}},
		{"生化危机：终章", "2017", []string{"米拉·乔沃维奇", "伊恩·格雷", "艾丽·拉特"}},
	}

	baseMovie := BaseResponse{200, "success", movies}

	//一个是不带缩进显示的，一个是带缩进显示的
	//data, err := json.Marshal(baseMovie)
	data, err := json.MarshalIndent(baseMovie, "", "    ")

	if err != nil {
		log.Fatalf("JSON marshaling failed: %s", err)
	}
	fmt.Printf(string(data))
}
```

## 输出部分
```
--------------数组测试---------------
[1 2 3 4]
[3 3 3 3]
--------------切片测试---------------
[一月 二月]
[二月 三月 四月]
[ 一月 二月 三月 四月 五月 六月 七月 八月 九月 十月 十一月 十二月]
[十二月 十一月 十月 九月 八月 七月 六月 五月 四月 三月 二月 一月 ]
[十一月 十月]
[十月 九月 八月]
--------------map测试---------------
map[3:world3 5:world5 6:world6 7:world7 9:world9 2:world2 4:world4 8:world8 10:!!!!!!10 1:hello1]
hello1
map[10:!!!!!!10 1:hello1 4:world4 8:world8 6:world6 7:world7 9:world9 3:world3 5:world5]
world41
--------------json测试---------------
{
    "Code": 200,
    "Msg": "success",
    "Data": [
        {
            "Title": "金刚狼",
            "Year": "2017",
            "Actors": [
                "休·杰克曼",
                "达芙妮·基恩",
                "帕特里克·斯图尔特"
            ]
        },
        {
            "Title": "极限特工",
            "Year": "2017",
            "Actors": [
                "范·迪塞尔",
                "露比·罗丝",
                "妮娜·杜波夫"
            ]
        },
        {
            "Title": "功夫瑜伽",
            "Year": "2017",
            "Actors": [
                "成龙",
                "李治廷",
                "张艺兴"
            ]
        },
        {
            "Title": "生化危机：终章",
            "Year": "2017",
            "Actors": [
                "米拉·乔沃维奇",
                "伊恩·格雷",
                "艾丽·拉特"
            ]
        }
    ]
}
```

# 在此感谢博客被百度、谷歌收录

![](http://oeqej1j2m.bkt.clouddn.com/google%E6%9F%A5%E8%AF%A2.JPG)

![](http://oeqej1j2m.bkt.clouddn.com/%E7%99%BE%E5%BA%A6%E6%9F%A5%E8%AF%A2.JPG)
