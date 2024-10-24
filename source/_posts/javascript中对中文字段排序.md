---
title: javascript中对中文字段排序
date: 2022-08-05 16:39:29
author: Evan
categories: 笔记
tags:
---

## 用javascript对数组中的中文字段进行排序

可以使用对象字面量来实现。

```
var o = [{id : 007, type : "广州"}, 
	 {id : 005, type : "深圳"},
	 {id : 008, type : "东莞"}, 
	 {id : 003, type : "中山"}, 
	 {id : 002, type : "深圳"}, 
	 {id : 006, type : "佛山"}];
var compareData = {
	"东莞" : 1,
	"佛山" : 2,
	"广州" : 3,
	"深圳" : 4,
	"顺德" : 5,
	"中山" : 6
};
l = o.sort(function compare(a,b){return compareData[a.type] - compareData[b.type];});
for(var i = 0; i < l.length; i++){
    document.write(l[i]["type"]);
}
```

