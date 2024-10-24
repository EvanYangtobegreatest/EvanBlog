---
title: 'Django使用Vue在html中按{{}}取值失败的解决办法'
date: 2024-02-22 17:12:06
author: Evan
categories: 笔记
tags:
---

因为django本身有模版的概念，直接取值是从django的模版中取值的

而在vue的v-for中想要获取遍历之后的数据，

直接用{{ var }}方式取值，会取到None

这时候就需要进行特殊处理，解决办法为：

**1、禁用Django的模版**

```html
{% verbatim %}
{% endverbatim %}
在需要不解析的DOM 上下 包裹 {% verbatim %} {% endverbatim %} 
例如：
<div id="app" v-cloak v-loading.fullscreen.lock="loading">
    {% verbatim %}
    <div class="subject" v-for="subject in subjects">
        <p class="sname">
            <a :href="'/teachers?sno=' + subject.no">
                {{ subject.name }}
            </a>
            <img v-if="subject.isHot" src=''>
        </p>
        <p>{{ subject.intro }}</p>
    </div>{% endverbatim %}
</div>
```

**2.更改Vue的中括号**

```js
//改成其他 例如："{[ ]}"等等，修改方式如下：
Vue.config.delimiters = ["{[", "]}"]
或者
new Vue({
  delimiters:['{[', ']}'],
  el: '.el',
  data:{}
 })
```
