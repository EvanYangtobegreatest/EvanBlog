---
title: 调用榛子云短信api服务
date: 2022-10-28 14:21:41
author: Evan
categories: 笔记
tags:
---

## Springboot调用榛子云短信api服务

[榛子云官方文档](http://smsow.zhenzikj.com/doc/ready.html)

#### 步骤

**1.添加依赖**

```xml
<!-- 榛子云api -->
        <dependency>
            <groupId>com.zhenzikj</groupId>
            <artifactId>zhenzisms</artifactId>
            <version>2.0.2</version>
        </dependency>
```

**2.编写控制类**

```
@Controller
public class Message {
    //短信平台相关参数
    //榛子云链接
    private String apiUrl = "https://sms_developer.zhenzikj.com";
    //榛子云系统上获取 填入自己账号的id和密钥
    private String appId = "";
    private String appSecret = "";
    @ResponseBody
    @GetMapping("/fitness/code")
    public boolean getCode(@RequestParam("memPhone") String memPhone, HttpSession httpSession){
        try {
            JSONObject json = null;
            //随机生成验证码
            String code = String.valueOf(new Random().nextInt(999999));
            //将验证码通过榛子云接口发送至手机
            ZhenziSmsClient client = new ZhenziSmsClient(apiUrl, appId, appSecret);
            Map<String, Object> params = new HashMap<String, Object>();
            params.put("number", "填自己的手机号码");
            params.put("templateId", "短信的模板id");
            String[] templateParams = new String[2];
            templateParams[0] = code;
            templateParams[1] = "5分钟";//设置有效时间
            params.put("templateParams", templateParams);
            String result = client.send(params);
            System.out.println(result);
            json = JSONObject.parseObject(result);
            System.out.println(result);
            //将验证码存到session中,同时存入创建时间
            //以json存放，这里使用的是阿里的fastjson
            json = new JSONObject();
            json.put("memPhone",memPhone);
            json.put("code",code);
            json.put("createTime",System.currentTimeMillis());
            // 将认证码存入SESSION
            redisTemplate.expire("verifyCode"+memPhone,1, TimeUnit.MINUTES);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }
}
```





