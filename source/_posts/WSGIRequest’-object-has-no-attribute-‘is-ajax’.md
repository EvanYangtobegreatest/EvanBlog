---
title: WSGIRequest’ object has no attribute ‘is_ajax’
date: 2024-02-21 15:19:02
author: Evan
categories: 笔记
---

### 中间件权限校验的Ajax请求判断的版本问题

使用中间件权限校验，判断是不是Ajax请求时产生报错：

```text
WSGIRequest’ object has no attribute ‘is_ajax’
```

django更新至4.0取消了request.is_ajax()这个判断；4.0版本的django可以使用以下代替：

```
request.headers.get('x-requested-with') == 'XMLHttpRequest':
```

也可以将django版本降低，就可以继续使用：

```python
request.is_ajax()
```

