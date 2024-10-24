---
title: 修改springboot内嵌tomcat上传文件大小
date: 2022-09-01 15:25:32
author: Evan
categories: 笔记
tags:
---

##  修改springboot内嵌tomcat上传文件大小

**错误信息:**

```cmd
The field file exceeds its maximum permitted size of 1048576 bytes.
```

**原因是因为SpringBoot内嵌tomcat默认所能上传的文件大小为1M,超出这个就会报错。**

### 解决办法

**1.修改application.yml配置文件**

```xml
spring:
  http:
    multipart:
      enabled: true
      max-file-size: 30MB
      max-request-size: 30MB
```

**2.编写配置类**

```java
package com.example.myspringboot.config;

import org.springframework.boot.web.servlet.MultipartConfigFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.util.unit.DataSize;

import javax.servlet.MultipartConfigElement;

@Configuration
public class MulterFile {
    /**
     * 文件上传配置
     * @return
     */
    @Bean
    public MultipartConfigElement multipartConfigElement() {
        MultipartConfigFactory factory = new MultipartConfigFactory();
        //文件最大
        factory.setMaxFileSize(DataSize.parse("30960KB")); //KB,MB
        /// 设置总上传数据总大小
        factory.setMaxRequestSize(DataSize.parse("309600KB"));
        return factory.createMultipartConfig();
    }
}
```

