---
title: git上传项目到gitee
date: 2022-12-26 11:36:30
author: Evan
categories: 笔记
tags:
---

### git上传项目到gitee

**1.下载安装gitee**

[git官网](https://git-scm.com/download/win)

**2.配置git的用户名和邮箱**

右键单击项目你要上传的项目，再点击**Git Bash Here**

然后输入命令:

```text
git init 
//初始化本地仓库,这个时候本地文件夹会生成一个.git文件，说明初始化成功了。
```

在项目里找到`.git`文件夹，点开后找到`config`文件打开。

```text
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
//添加user这一项，名字邮箱填自己的
[user]
	name = @yangyewen
	email = 1310244108@qq.com
```

可以通过以下操作看下是否成功设置名字邮箱

```text
//查看用户名
git config user.name
//查看邮箱
git config user.email

//设置用户名
git config --global user.name "xxx"
//设置邮箱
git config --global user.email "xxx"
```

**3.上传到远程仓库**

打开gitee里要上传的项目，复制项目地址

>  ***克隆下载--->HTTPS--->复制***

打开命令框，输入命令：

```text
git remote add origin 上一步复制的链接
```

**4.将远程origin主机的master分支拉取过来和本地的当前分支进行合并**

输入命令：

```text
git pull origin master
```

**5.将当前目录下修改的所有代码从工作区添加到暂存区 . 代表当前目录**

输入命令：

```text
git add .
```

**6.将缓存区内容添加到本地仓库**

输入命令：

```text
git commit -m '说明' //说明处是指提交时的备注，例如git commit -m '第一次提交’
```

**7.将本地版本库推送到远程服务器**

输入命令：

```text
git push origin master
//强制推送 git push -f origin master
```

最后刷新gitee仓库 成功！
