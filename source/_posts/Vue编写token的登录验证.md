---
title: Vue编写token的登录验证
date: 2022-06-27 17:52:34
author: Evan
categories: 笔记
tags: 
---

## 实现Vue根据token拦截访问页面

### 实现步骤

1.前端登录，把用户信息传到后端。

2.后端验证通过后生成token返回前端。

3.前端获取token后用vuex和localStorage管理，进入其他页面。

4.之后前端每一次权限操作如跳转路由，都需要判断是否存在token，若不存在，跳转至登录页。

5.前端的请求操作都要在请求头带上token，后端验证请求头有token之后才开放接口。

6.如果token已过期，清除token信息，跳转至登录页。



## vue实现

**1.获取后端传来的token**

```js
login () {
                this.$axios
                    .post('/login', this.$qs.stringify({name: this.loginForm.name,
                        password: this.loginForm.password
                    }))
                    .then(successResponse => {
                        if (successResponse.data.code === 200) {
                            localStorage.setItem('token',successResponse.data.data);//存储token信息
                            this.$router.replace({path: '/dashboard'})
                        }
                        else{
                            alert(successResponse.data.message);
                        }
                    })
                    .catch(failResponse => {

                    })
            }
```

**2.在路由中添加拦截**

requireAuth属性作用是表明该路由是否需要登录验证，在进行全局拦截时，我们将通过该属性判断路由的跳转，该属性包含在meta属性中:

```js
routes = [
    {
        name: 'dashboard',
        path: '/dashboard',
        meta: {
            requireAuth: true
        }
    },
    {
        name: 'login',
        path: '/login'
    }
]
```

**3.在router/index.js添加路由守卫**

```js

// 使用 router.beforeEach 注册一个全局前置守卫，判断用户是否登陆
router.beforeEach((to,from,next)=>{
    //to 将要访问的路径
    //from 代表从哪个路径而来
    //next() 代表放行 next('xxx') 强制放行的xxx的路径
    if(to.path==='/'||to.path==='/Login'){
        next();
    }else{
        const tokenStr=window.localStorage.getItem('token')
        if(!tokenStr){
            return next('/Login')
        }
        next()
    }
})
```

**4.在main.js设置token请求头**

```js
//将token添加到axios请求头部
axios.interceptors.request.use(
    config => {
        if (localStorage.getItem('token')) {
            config.headers.token = localStorage.getItem('token')
        }
        return config
    },
    error => {
        return Promise.reject(error)
    }
)

```

**5.结合后端写好的token方法即完成**
