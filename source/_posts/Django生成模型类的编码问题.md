---
title: Django生成模型类的编码问题
date: 2024-01-10 16:42:19
author: Evan
categories: 笔记
tags:
---





## Django生成模型类的编码问题

利用Django的ORM，将数据表变成Django中的模型类。

```python
python manage.py inspectdb > 应用名/models.py
```

有了Django框架的ORM，我们可以直接使用面向对象的方式来实现对数据的CRUD（增删改查）操作。我们可以在PyCharm的终端中输入下面的命令进入到Django项目的交互式环境，然后尝试对模型的操作。

```
python manage.py shell
```

然后会报错误：

```
ValueError: source code string cannot contain null bytes
```

## 解决办法

该问题是因为生成的文件编码问题，将models.py的内容改成UTF-8格式即刻解决
