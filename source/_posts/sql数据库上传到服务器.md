---
title: sql数据库上传到服务器
date: 2022-06-16 16:40:35
author: Evan
categories: 笔记
tags:
---

## 操作准备

Xshell、Xftp、已连接服务器。

已安装mysql的服务器。

## 操作步骤

**1.将要上传的数据库导出为sql文件，保存到本地中。可以通过Navicat等sql工具导出，也可以本地电脑CMD命令导出。**

**2.通过Xftp、Xshell连接远程服务器。**

**3.将步骤1中保存的本地sql文件通过Xftp上传到服务器的文件夹中(路径随意)，记住这个上传路径。(例：/usr/mydatabase/shuju.sql)**

#### 服务器中的操作

**4.在Xshell中输入mysql -u root -p,输入正确密码进入服务器数据库中；**

```mysql
mysql -u root -p
```

**5.进入mysql后创建一个数据库，名字与导入的sql文件一致；**

```mysql
create database shuju;
```

**6.退出mysql**

```mysql
exit;
```

**7.执行命令sudo mysqldum完整如下**

```Linux
//sudo mysqldump -u root -p '数据库名' < 'sql文件的路径';
sudo mysqldump -u root -p shuju < /usr/mydatabase/shuju.sql;
```

**8.进入mysql，执行use shuju**

```mysql
mysql -u root -p;
use shuju;
```

**9.然后执行source操作,source+sql路径**

```mysql
 source  /usr/mydatabase/shuju.sql;
```

**到这里，已经完成了数据库的上传。**

