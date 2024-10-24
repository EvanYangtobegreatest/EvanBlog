---
title: xml导出word图片不显示问题
date: 2022-06-23 15:23:15
author: Evan
categories: 笔记
tags:
---

## xml导出word图片不显示问题

最近写个需求，要求打印出word形式的报告文档。其中出现了使用officeword文档打开后图片没有正常显示。

![](/img/随笔/图片显示失败.PNG)

但是使用wps却可以正常显示。

## 解决方法

`<w:binData>` 与 `<\binData>` 标签之间不能有其他任何字符。

原因是我格式化文档时，代码工具把这个标签换行了，所以多了个回车符。

**错误示范**

![](/img/随笔/xml图片问题.PNG)

**正确写法**

![](/img/随笔/xml图片问题2.PNG)
