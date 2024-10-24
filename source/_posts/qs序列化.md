---
layout: vue
title: qs序列化
author: Evan
categories: 笔记
date: 2022-06-13 17:14:55
tags:
---

## vue使用axios结合qs进行序列化

开发vue+elementUI项目时，使用axios进行前后端分离，会出现发送请求后端接收不到，原因在于后端接受的数据类型和axios发送的请求参数类型不一致，例如springboot中controller中的参数类型是字符串，而axios的请求参数是json格式。这时候就需要把参数类型设为一致。

在vue的可以使用qs来对请求参数进行序列化。

### 使用方法

**安装**

```
npm install qs
```

**引用**

在main.js中全局引用

```vue
import qs from'qs'
//全局引用
Vue.prototype.$qs = qs
```

**使用**

```js
//例子 
this.$axios
                    .post('/login', this.$qs.stringify({name: this.loginForm.name,
                        password: this.loginForm.password
                    }))
                    .then(successResponse => {
                        if (successResponse.data.code === 200) {
                            localStorage.setItem('token',successResponse.data.data);
                            this.$router.replace({path: '/dashboard'})
                        }
                        else{
                            alert(successResponse.data.message);
                        }
                    })
                    .catch(failResponse => {

                    })
```

### qs的两种方法

**qs.parse()**

```
qs.parse()是将URL解析成对象的形式
//例
const url = 'name=admin&password=123'
qs.parse(url) // 返回数据  {name:admin,password:123}
```

**qs.stringify()**

```
qs.stringify()将对象 序列化成URL的形式以&进行拼接
//例
const obj = {
	name: 'admin',
	password: '123'
}
qs.stringify(obj) // 返回数据：name=admin&password=123
```

