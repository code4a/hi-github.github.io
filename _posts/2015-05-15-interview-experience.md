---
layout: post
title: 面试中学习
categories: [experience]
---

# 在面试中成长

> 最近因为意识到目前工作内容的局限性，有一定换工作的意向，然后就更新了一下自己的简历，基本上也没过多久，就接收到了一下面试邀请，自己也是相通过面试来对这将近一年的工作的一个检验，同样是想在面试中去学习一些可能自己曾经不曾接触的知识面，从中也汲取一些对自己有帮助的信息。对今天的两场面试做一些简单回顾，对其中的一些知识点做些笔记，懂得不懂得，可以先记录下来，慢慢的将不懂得也能掌握到。

### 做技术的不能过的太安逸

> 今天的一场面试中，被面试官问到都会些什么语言。我说到会java、c/c++、jekyll、javascript、html、css和Android相关的语言，当被面试官问道会object-c吗？知道h5吗，会写脚本吗？等等，而我的答案明显是no，然后就得到了面试官的评论，接触的面太窄了，眼光也没那么长远诸如此类的评价，作为一个开发者不能局限与一个层面，一个方向。让我意识到自己之前可能确实过的有些安逸和虚度，不能说上是一个忠实的技术者，少了对技术的狂野和究其原理的意识和耐心，所以觉得有必要对这次面试中涉及到的东西记录下，以便时间长遗忘，而忘记去做深入的了解

### apk常用的打包方式

* Eclipse工程中右键工程，弹出选项中选择 android tools-生成签名应用包，根据流程使用私钥，输入密码，保存即可
* 使用ANT打包apk，其一般会经过以下几个步骤：
	* 用aapt命令生成R.java文件
	* 用aidl命令生成相应java文件
	* 用javac命令编译java源文件生成class文件
	* 用dx.bat将class文件转换成classes.dex文件
	* 用aapt命令生成资源包文件resources.ap_
	* 用apkbuilder.bat打包资源和classes.dex文件,生成unsigned.apk
	* 用jarsinger命令对apk认证,生成signed.apk

下面是流程图：

![android_ant_make_apk_eg_pic]({{ site.url }}/images/posts/android_ant_make_apk_eg_pic.png)

此种方式详细的可以参考**[android应用打包成apk的两种方式](http://blog.csdn.net/sugarsla/article/details/8239491)**

### apk的热更新

### jar和dex放到远端服务器哪种方式的更新更有优势

### 如何搭建vpn

### 翻墙工具使用的协议

### 开发中常用的设计模式

### android的进程被强制杀死，如何在被杀前做收尾工作

### android程序的入口

### application和mainActivity的区别

### Sqlite是否支持事务

### MySql的关系型索引和索引

### 获取到apk的bin目录能否利用其中的资源打包成apk
