---
title: <meta>标签name=viewport详解
date: 2022-04-02 17:44:17
author: Evan
index_img: /img/bg8.jpg
categories: 笔记
tags:
---



# `<meta>`标签 name="viewport" 详解



今天给老板做的驾驶舱基本完成了，准备投到电视机大屏上看下效果，结果出现页面显示大小不对，于是看看是不是分辨率的问题，最后发现了我html页面的`<meta>`标签设置了不能缩放，最后修改成下面语句，成功显示。

```html
<meta name="viewport" content="width=1680"> 
```

记个笔记...

#### 什么是Viewport

通俗的讲，移动设备上的viewport就是设备的屏幕上能用来显示我们的网页的那一块区域，在具体一点，就是浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。在默认情况下，一般来讲，移动设备上的viewport都是要大于浏览器可视区域的，这是因为考虑到移动设备的分辨率相对于桌面电脑来说都比较小，所以为了能在移动设备上正常显示那些传统的为桌面浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或1024px（也可能是其它值，这个是由设备自己决定的），但带来的后果就是浏览器会出现横向滚动条，因为浏览器可视区域的宽度是比这个默认的viewport的宽度要小的。下图列出了一些设备上浏览器的默认viewport的宽度。

#### Viewport 基础

一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下：
 `<meta name=”viewport” content=”width=device-width, initial-scale=1, maximum-scale=1″>`
 `width`：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）
 `height`：和 width 相对应，指定高度
 `initial-scale`：初始缩放比例，也即是当页面第一次 load 的时候缩放比例
 `maximum-scale`：允许用户缩放到的最大比例
 `minimum-scale`：允许用户缩放到的最小比例
 `user-scalable`：用户是否可以手动缩放

下面的一行代码可以让网页的宽度自动适应手机屏幕的宽度:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```





