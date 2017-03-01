---
title: Git使用笔录
date: 2016-10-29 13:09:43
tags: [Git, 版本管理]
---

## git工具下载：[https://git-scm.com/download/](https://git-scm.com/download/)
**配置git信息**

```
   git config --global user.name "Your name"
   git config --global user.email "Your email"
```

<!--more-->

# 1、git本地仓库初始化

```
    git init
    git remote remove/add origin git仓库地址
    git pull origin master master //取远程主分支
    git pull origin Liang Liang //取远程Liang的分支
```

# 2、提交远程到仓库

```
    git add .
    git commot -m "提交信息"
    git push origin MoLiang //提交到远程分支MoLiang
```

# 3、分支管理

```
    git branch Liang //创建Liang的分支
    git branch origin/Liang //创建Liang的远程分支
```

# 4、合并主分区

```
    git checkout master (主分支)
    git merge origin/MoLiang
    git push origin master:master //$ git push <远程主机名> <本地分支名>:<远程分支名>
```

# 5、回滚

## 主分支

```
    $ git log 查看commit的sha1值+
    $ git reset 3a48e096339f5c840a8d15435ff88cbd11ccd31f
    $ git stash
    $ git push -f origin master
```