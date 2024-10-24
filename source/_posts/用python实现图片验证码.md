---
title: 用python实现图片验证码
date: 2024-01-24 16:42:19
author: Evan
categories: 笔记
tags:

---





# 用python实现图片验证码

**1.安装模块pillow**

```python
#安装pillow模块
pip3 install pillow
```

**2.生成随机验证码**

```python
def gen_random_code(length=4):
    ALL_CHARS = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
    return ''.join(random.choices(ALL_CHARS, k=length))
```

**3.根据验证码生成图片**

```python
def generate_image(code, width, height, font_size):
    # 创建一个新的图片对象
    img = Image.new(mode='RGB', size=(width, height), color=(255, 255, 255))
    # 创建一个画笔对象
    draw = ImageDraw.Draw(img)
    # 设置字体
    font = ImageFont.truetype('arial.ttf', font_size)
    # 获取字符的宽度和高度
    # text_width, text_height = draw.textsize(code, font)
    # 获取字符的宽度和高度
    bbox = draw.textbbox((0, 0, width, height), code, font=font)
    text_width = bbox[2] - bbox[0]
    text_height = bbox[3] - bbox[1]

    # 将字符绘制在图片中央
    x = (width - text_width) // 2
    y = (height - text_height) // 2
    draw.text((x, y), code, font=font, fill=(0, 0, 0))
    # 添加干扰线
    for i in range(5):
        x1 = random.randint(0, width)
        y1 = random.randint(0, height)
        x2 = random.randint(0, width)
        y2 = random.randint(0, height)
        draw.line((x1, y1, x2, y2), fill=(0, 0, 0), width=2)
    # 添加干扰点
    for i in range(50):
        x = random.randint(0, width)
        y = random.randint(0, height)
        draw.point((x, y), fill=(0, 0, 0))
    # 返回验证码图片对象
    return img
```

**调用**

```python
def get_captcha(request: HttpRequest) -> HttpResponse:
    """验证码"""
    captcha_text = gen_random_code()
    request.session['captcha'] = captcha_text
    image_data = generate_image(request.session['captcha'], 150, 50, 30)
    //保存图片
    save_image(image_data, 'captcha.png')
    //展示图片
    image_data.show()
    return HttpResponse(image_data, content_type='image/jpeg')
```

