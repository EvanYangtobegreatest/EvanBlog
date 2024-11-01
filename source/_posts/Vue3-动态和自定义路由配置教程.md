---
layout:
  - page
title: Vue3 动态和自定义路由配置教程
date: 2024-11-01 16:05:16
tags:
---

# Vue 3 动态和自定义路由配置教程

本教程展示了如何在 Vue 3 项目中，通过动态导入组件的方式自动注册 `components` 文件夹下的所有组件为路由页面，并允许手动添加自定义路由。

## 先决条件

- 已安装 Node.js (v14+)
- Vue 3 项目（可通过 `vue create my-project` 创建）
- 已安装 Vue Router（如未安装，可运行 `npm install vue-router`）

## 实现步骤

### 1. 配置路由

在 `src/router` 目录中创建 `index.js` 文件，并将以下代码粘贴到文件中：

```javascript
// src/router/index.js
import { createRouter, createWebHistory } from 'vue-router'

// 动态导入 src/components 文件夹下的所有 Vue 文件
const componentFiles = import.meta.glob('../components/*.vue')

// 动态生成路由
const routes = Object.keys(componentFiles).map((filePath) => {
    const name = filePath.split('/').pop()?.replace('.vue', '') // 使用文件名作为路由名称
    return {
        path: `/${name?.toLowerCase()}`, // 将路径转换为文件名的小写形式
        name: name,
        component: componentFiles[filePath], // 动态加载组件
    }
})

// 自定义路由
const customRoutes = [
    {
        path: '/',
        name: 'index',
        component: () => import('~/components/index.vue'), // 自定义首页
    },
    {
        path: '/about',
        name: 'about',
        component: () => import('~/components/about.vue'), // 自定义 About 页面
    }
]

// 合并动态路由和自定义路由
const finalRoutes = [...routes, ...customRoutes]

// 创建路由实例
const router = createRouter({
    history: createWebHistory(),
    routes: finalRoutes,
})

export default router
```

### 2. 注册路由实例

在 `main.js` 文件中导入并注册路由实例：

```javascript
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App)
    .use(router)
    .mount('#app')
```

### 3. 添加导航和测试路由

在 `App.vue` 中添加导航链接，方便访问动态路由和自定义路由：

```html
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/example">Example</router-link> <!-- 自动生成的路由 -->
      <router-link to="/about">About</router-link>     <!-- 自定义的路由 -->
    </nav>
    <router-view />
  </div>
</template>
```

### 4. 运行项目

运行项目并在浏览器中访问以下路径，测试是否生效：

- `/` - 自定义的首页
- `/about` - 自定义的 About 页面
- `/example` - `components` 文件夹下 `Example.vue` 组件自动生成的路由

## 常见问题

### 如何为特定路由添加 `meta` 信息？

可以在动态路由生成后，为特定路由添加 `meta` 信息：

```javascript
routes.forEach(route => {
  if (route.name === 'Example') {
    route.meta = { requiresAuth: true }
  }
})
```

### 如何在路径中使用别名？

确保在 `vite.config.js` 或 `vue.config.js` 中配置路径别名，例如 `~/components`：

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '~': path.resolve(__dirname, 'src')
    }
  }
})
```
