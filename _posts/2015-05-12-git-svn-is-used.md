---
layout: post
title: git/svn常用命令
categories: [version control]
---

# 版本控制工具

> 现在公司的项目基本都会用到 git 或者 svn 这样的工具对代码进行管理，所以掌握它们也是很有必要的。好记性不如烂笔头，还是将常用的命令记录下来，以便忘记时查看一下

### git 常用命令

* 克隆远程代码库

		git clone email:/reponame.git 或 git clone https://github.com/hi-github/material-dialog.git

* 更新源码
		
		git pull

* 分支相关

		git branch 	--查看分支
		git checkout -b name 	--创建分支

* 更新相关

		git add .    --增加新的内容
		git commit -a -m 'commit msg'   --提交所有修改，并描述修改内容
		git push -u origin master    --将内容提交到主分支

* Git状态

		git status

* 查看提交内容的差异

		git log -p -1    (1 是最新的一条)

### Svn 常用命令

* 将文件下载到本地

		svn checkout path(服务器上的目录)
		//简写 svn so

* 往版本库添加新文件

		svn add file(文件名)

* 提交文件到版本库

		svn commit -m 'logMassage' （文件名）

* 加锁/解锁

		svn lock -m 'lockMessage' （文件名）
		svn unlock path

* 更新到某个版本

		svn update -r m path    -- m是版本号， path为文件名
		// 简写 svn up

* 查看文件或者目录状态

		svn status path

* 删除文件

		svn delete path -m 'delete message'
		// 简写 svn (del, remove, rm)

* 查看日志

		svn log path

* 比较差异

		svn diff 文件名
		// 简写 svn di
