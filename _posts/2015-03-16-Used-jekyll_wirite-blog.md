---
layout: post
title: Jekyll基本使用
categories: [Jekyll]
---

## Jekyll
> 前一段时间从朋友那里看到他写的博客感觉很有Big，虽就问他怎么弄得，然后就了解到了Jekyll，然后就各种百度(公司的网络用的是代理，常识各种翻墙，各种失败)，最后决定按照 **[天镶的博客](http://segmentfault.com/blog/skyinlayer/1190000000406011)** 里边写的过程去试着配置环境，整个操作下来还是比较简单的，过程中有参照 **[oukongli的专栏](http://blog.csdn.net/kong5090041/article/details/38408211)** ，因为公司这边的网络实在是不敢恭维CSDN博客也只能用快照浏览。参照两位大牛的流程搭建环境就差不多啦。

---

## 博客模板
	
> 博客的模板我是在Github上边Fork了 [袁勇](willard-yuan/willard-yuan.github.io) 的Repository，然后Clone到本地做了简单的修改，模板都是看个人的爱好和需要可以自己查询，修改好之后可以通过 jekyll server 命令启动服务，然后通过localhost:4000预览一下效果，如果觉得可以，就可以提交到github啦。

---

## 本地预览
> 我是使用的MarkdownPad写的笔记，笔记中基本都是中文，然后通过 jekyll server 启动成功之后，打开预览是发现中文都乱码啦
，
## 代码提交
	按照上面所说的步骤是需要新建 gh-pages 分支的，需要将blog提交到该分支，因为我这边之前已经有了GitHub Pages默认创建的一个
	模板，并且在master分支上，提交到 gh-page 分支之后，通过固定格式的仓库名访问的仍旧是master分支上的默认模板，然后就问了
	一下朋友需要将master分支给删除掉，然后重新在Commit一次，因为之前修改的模板不在同一台电脑所以就重新clone了一遍，然后重新
	再操作一次，整个过程其实也并不复杂，对于程序员来说这些东西都是So Easy
