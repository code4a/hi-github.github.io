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
> 我是使用的MarkdownPad写的笔记，笔记中基本都是中文，然后通过 jekyll server启动成功之后，打开预览是发现中文都乱码啦,后来仔细看下原来是笔记没有按照yaml的格式去写，所以一定要注意开头的格式，如果实在不知道可以参照这篇日记得格式
，
## 代码提交
>按照上面所说的步骤是需要新建 gh-pages分支的，需要将blog提交到该分支，但是我按照上面的方式试了三次都没有成功，后来就把这个gh-pages分支删除掉了，移除了原来master分支的所有文件，然后将原来写好的模板重新提交到master分支之后，稍等片刻静态的博客就成功的创建了。
