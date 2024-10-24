---
title: 'Vue 报错error:0308010C:digital envelope routines::unsupported'
date: 2024-03-12 15:39:11
author: Evan
categories: 笔记
tags:
---

### 错误代码 "error:0308010C:digital envelope routines::unsupported" 是由 OpenSSL 库引起的，它通常与加密相关的操作有关。

**这个错误通常在使用 Vue.js 或其他使用 OpenSSL 的应用程序时出现。它可能是由以下几个原因引起的：**

1.OpenSSL 版本不兼容：某些版本的 OpenSSL 可能与你正在使用的应用程序不兼容。尝试更新 OpenSSL 版本或检查应用程序的最低要求。

2.缺少所需的加密算法：有时候，应用程序可能需要特定的加密算法，但 OpenSSL 可能没有提供该算法支持。在这种情况下，你可以尝试更新 OpenSSL 版本，以获取所需的算法支持。

3.系统环境配置错误：在某些情况下，系统环境配置可能会导致 OpenSSL 出现问题。确保你的系统环境正确配置，以便应用程序能够正确使用 OpenSSL。

#### 最佳解决方法：package.json增加配置

```js
"scripts": {
    "serve": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
    "build": "vue-cli-service build"
  }
```

文章转至：https://blog.csdn.net/m0_63324772/article/details/131082841
