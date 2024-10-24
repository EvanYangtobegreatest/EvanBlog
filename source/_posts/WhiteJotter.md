---
title: WhiteJotter
date: 2022-03-25 09:11:24
author: Evan
index_img: /img/bg7.jpg
categories: 笔记
tags:
- vue
- springboot
---



# 引言

在csdn上看到一个vue+springboot的项目实战教程很不错：https://learner.blog.csdn.net/article/details/88925013，

跟着做一下，锻炼一下自己。本文旨在记录下开发过程中遇到的问题和解决方法。



 

## 1.

如果前端打包之后dist文件夹下面的文件copy到后端static目录下 ，访问` http://localhost:8080/index.html` 这个地址，没有出现作者说的获取到一个空白页面而是出现了404报错，可以在idea中，找到项目结构（快捷键：ctrl+alt+shift+s），选择工件，添加一个空的JAR类型的工件，再重新运行项目，访问`http://localhost:8080/index.html`这个地址会正常出现作者说的空白页面，`http://localhost:8080/login`，如果没有设置后端ErrorConfig,会出现作者说的错误页，设置后则会正常出现登录界面。

## 2.

### 报错

```
//引入vuex的时候报错
D:\study\ey-vue>npm install vuex --save
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR!
npm ERR! While resolving: my-project@1.0.0
npm ERR! Found: vue@2.6.14
npm ERR! node_modules/vue
npm ERR!   vue@"^2.5.2" from the root project
npm ERR!
npm ERR! Could not resolve dependency:
npm ERR! peer vue@"^3.0.2" from vuex@4.0.2
npm ERR! node_modules/vuex
npm ERR!   vuex@"*" from the root project
npm ERR!
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force, or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR!
npm ERR! See C:\Users\Evan\AppData\Local\npm-cache\eresolve-report.txt for a full report.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Evan\AppData\Local\npm-cache\_logs\2022-03-24T09_36_10_865Z-debug-0.log

```

解决方法：

```
npm install vuex@3.6.2 -S
```

## 3.

**报错:**

添加修改图书时，选择分类，只有第一次点击才有图书分类,并且console报错。

**错误原因**

editForm里面的clear()有问题，执行之后会造成category的属性丢失，从而导致连续添加图书的时候，第二次会出现无法选择图书分类，而且console报错

```javascript
clear () {
      this.form = {
        id: '',
        title: '',
        author: '',
        date: '',
        press: '',
        cover: '',
        abs: '',
       //原因：
          category: ''
      }
   },
```

**解决方法**

```js
   //可以将clear中category属性赋空，而不是将category赋值为空
   category: {
                id: '',
                name: ''
              }
```

## 4.

**出错：**

 教程添加“文件上传”的模块后，没有“点击上传”按钮，

**解决方法**

```js
import ImgUpload from './ImgUpload'
      export default {
        name: 'EditForm',
        components: {ImgUpload},//导入时还要加入这一行代码
```

