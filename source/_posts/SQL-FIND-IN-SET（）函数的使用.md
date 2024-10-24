---
title: SQL-FIND_IN_SET（）函数的使用
date: 2022-08-23 09:34:55
author: Evan
categories: 笔记
tags:
- MySql
---

## FIND_IN_SET()函数

有这样的一个使用场景，当我需要从一个表中查询出某列的值符合一个条件集的数据。

如果条件集数量确定的话，可以使用in进行查询，比如说固定条件是3个的时候：

user_name  = 杨叶文 or  user_name  = 刘德华  or user_name  = 彭于晏。

```sql
select * from user where user_name in ('杨叶文','刘德华','彭于晏');
可以在in(@var1,@var2,@var3)加三个参数，然后按得到的值分别赋给 @var1,@var2,@var3。
select * from user where user_name in ( @var1,@var2,@var3);
```

那么但如果传入的条件集是一个变量的话，传入 in里面的值数量不确定的时候,例如用户这次要查询3个人数据，下次要查询6个人的数据，那么固定参数数量的方法就无法使用了。

当传入一个大小不确定的list集合，或者传入一个长度不一定的字符串，那么in方法就无法使用。

这时可以使用FIND_IN_SET()函数

```sql
FIND_IN_SET(str,strlist) 
str 要查询的字符串
strlist 字段名 参数以”,”分隔 如 (1,2,6,8)
查询字段(strlist)中包含(str)的结果，返回结果为null或记录
```

实现：

```sql
select * from user where FIND_IN_SET(user_name,@nameStr);
nameStr:"杨叶文,刘德华,彭于晏";
```

​										
