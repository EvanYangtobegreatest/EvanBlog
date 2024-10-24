---
title: mybatis动态字段传值问题
date: 2022-09-01 17:35:10
author: Evan
categories: 笔记
tags:
---

## mybatis动态字段传值问题

开发时有这样一个需求，sql语句的字段根据传值查询

一般都会这样写

```sql
 select * from  Student where #{fieldname} like "%"#{keywords}"%"
```

但这条sql语句查出来的结果为0；

原因在于#{}为了防止sql注入都会给传入的参数加一个双引号，也就是上面的sql语句实际是这样的：

```sql
fieldname:"name",keywords="杨"
select * from  Student where "fieldname"  like "%杨%"
```

数据库找不到字段 "fieldname" ，所以查询结果为0；

所以这种情况下就可以用到${},${}将传入的参数直接显示生成在sql中，不会添加引号。

```sql
fieldname:"name",keywords="杨"
select * from  Student where ${fieldname} like "%"#{keywords}"%" 
生成为
select * from  Student where name like "%杨%"
```

所以#{}可以防止sql注入，在我们确定传入的值是安全的情况下，例如传的变量字段，那么就可以使用${}。
