---
title: GitHub使用Hexo搭建博客与版本控制
tags: [GitHub Pages, Hexo, 版本管理]
---
## 一. 本地准备工作
1、安装Node.js、Git
<!--more-->
## 二. 创建github仓库（以以下规则为准）
1、仓库名：github名.github.io（lianghuiyong.github.io）

2、创建时勾选**Initialize this repository with a README**，会自动创建master分支

3、创建仓库分支hexo
  1) 本地新建一个文件夹test
  2) test文件夹里鼠标右键选择Git bash，输入以下指令：
  ```
  git init
  git remote add origin https://github.com/github名/github名.github.io.git
  git pull origin master
  git branch hexo
  git checkout hexo
  git push origin hexo
  ```

[git常用命令](https://lianghuiyong.gitbooks.io/git-note/content/chapter1.html "查看git常用命令")
```
分支master（展示网页的分支必须是master），用于放hexo生成的静态网站文件。
分支hexo（这个可以随意取），用于存储hexo文件。
```
## 三. 第一次初始化hexo项目（不是从自己的github仓库下载过来的hexo工程文件）
**注：本地电脑要与github通过验证，git提交协议有ssh，HTTPS等验证方式，我用的HTTPS。**

在自己创建的博客目录里鼠标右键选择Git bash，输入以下指令：
1、获取hexo工程目录
```
npm install -g hexo-cli
```
2、初始化hexo，现在我感觉就是初始化hexo工程根目录下的_config.yml：
```
hexo init
```
3、下载主题，默认的没用过，我用的yilia
```
git clone https://github.com/litten/hexo-theme-yilia.git  themes/yilia
```
4、配置主题，打开hexo根目录下的_config.yml：
theme: yilia （注意：后有个空格）

5、在上面打开的_config.yml文件，配置hexo生成的静态网页提交仓库地址（注意空格）
```
deploy:
  type: git
  repo: https://github.com/github名/github名.github.io.git
  branch: master
```
6、生成gitahub静态博客
```
hexo clean
hexo g
hexo d
```
7、在浏览器上打开 http://github名.github.io.git ，看看有没有效果

## 四. 项目目录搭建好后，将hexo工程目录提交到github的hexo分支
1、将hexo工程下的**.deploy_git**删除

2、进themes/yilia把** .git**隐藏目录删除

3、进到hexo工程根目录，初始化git
```
git init
```
4、添加github仓库地址
```
git remote add origin https://github.com/github名/github名.github.io.git
```
5、获取github的hexo分支
```
git pull origin hexo
```
6、本地切换到hexo分支
```
git checkout hexo
```
7、添加hexo工程目录到git新节点
```
git add .
git commit -m "updata hexo"
```
8、提交新节点到github仓库
```
git push origin hexo
```
## 五. 初始化项目目录后或者从github的hexo分支获取到hexo工程目录
附：从hithub获取hexo目录

```
1）、创建一个博客文件夹，进去并初始化
	git init
	git remote add origin https://github.com/github名/github名.github.io.git
2）、获取hexo分支目录
	git pull origin hexo
```

1、写静态博客
```
hexo new “***”
hexo g
hexo d
```

# 拓展
**使用Android Studio等编译器来版本控制博客项目文件**

![Git忽略文件](http://oeqej1j2m.bkt.clouddn.com/hexo_1111.png "Git忽略文件")

![示例](http://oeqej1j2m.bkt.clouddn.com/hexo_2222.png "示例")

git config core.longpaths true
