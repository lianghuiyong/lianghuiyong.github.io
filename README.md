
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=455502271&auto=0&height=66"></iframe>

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
  <meta name="baidu-site-verification" content="U4oFD5dyt1" />
```

为了Google验证问题，我还把Html验证文件加进了主题目录下source目录，然后手动添加roborts.txt文件

## 百度自动推送

直接添加到主题目录下的layout/post.swig文件下
```
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
```

# RSS
安装插件： npm install hexo-generator-feed -save

plugins:
- hexo-generator-sitemap
- hexo-generator-feed

menu:
  Home: /
  Archives: /archives
  Rss: /atom.xml