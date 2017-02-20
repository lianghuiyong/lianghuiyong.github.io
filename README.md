# Hexo 笔记
[[博客推广——提交搜索引擎] ](http://selfboot.cn/2014/12/21/add_blog_to_google/)
[[搭建参考] ](http://www.jianshu.com/p/35e197cb1273)

# Git 笔记
**1、git pull  出错：Filename too long**

fatal: cannot create directory at 'node_modules/hexo-deployer-git/node_modules/hexo-fs/node_modules/chokidar/node_modules/anymatch/node_modules/micromatch/node_modules/braces/node_modules/expand-range/node_modules/fill-range/node_modules/isobject/node_modules/isarray': Filename too long

```
git config --global --add core.longpaths true
```

# Google搜索验证
在对应主题的layout/_partial/head.ejs文件中添加Google元标记

安装插件：npm install hexo-generator-sitemap

plugins:
- hexo-generator-sitemap

```
<head>
  <meta charset="utf-8">
  <meta name="google-site-verification" content="iK62VKwEv6LkAh49k31SAJZdpqWSPsnaXAalp1Ohtn8" />
```

为了Google验证问题，我还把Html验证文件加进了根目录下，然后手动添加roborts.txt文件

# RSS
安装插件： npm install hexo-generator-feed -save

plugins:
- hexo-generator-sitemap
- hexo-generator-feed

menu:
  Home: /
  Archives: /archives
  Rss: /atom.xml