---
title: JSON对象数组转Java对象数组
date: 2022-09-06 14:38:08
author: Evan
categories: 笔记
tags:
---

## JSON对象数组转Java对象数组

**引入阿里的fastjson**

```xml
 <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.60</version>
    </dependency>
```

**创建对应JSON数据的实体类**

可以用JSON工具一键生成实体类代码

```java
public class JsonRootBean {

    private String Name;
    private String SEX;
    private String age;
    public void setName(String Name) {
         this.Name = Name;
     }
     public String getName() {
         return Name;
     }

    public void setSEX(String SEX) {
         this.SEX = SEX;
     }
     public String getSEX() {
         return SEX;
     }

    public void setAge(String age) {
         this.age = age;
     }
     public String getAge() {
         return age;
     }
@Override
    public String toString() {
        return "JsonRootBean{" +
                "SampleName='" + SampleName + '\'' +
                ", Marker='" + Marker + '\'' +
                ", Peak1='" + Peak1 + '\'' +
                ", Peak2='" + Peak2 + '\'' +
                '}';
    }
}
```

**编写生成方法**

记得修改相关类名

```java
  private static ArrayList<JsonRootBean> getPlatformList(String arrStr) {
        ArrayList<JsonRootBean> jsonContents = new ArrayList<JsonRootBean>();
        JSONArray jsonContentList = JSON.parseArray(arrStr);
        for (Object jsonObject : jsonContentList) {
            JsonRootBean jsonContent = JSONObject.parseObject(jsonObject.toString(), JsonRootBean.class);
            jsonContents.add(jsonContent);
        }
        return jsonContents;
    }
```

**调用方法**

```Java
  public static void main(String[] arg) {
  String jsonStr = "[{
        "Name": "YYW",
        "SEX": "男",
        "age": "17"
},
    {
        "Name": "JAVA",
        "SEX": "男",
        "age": "22"
}
]"
//调用getPlatformList方法
  ArrayList<JsonRootBean> platformList = getPlatformList(jsonStr);
//检验结果
   for (JsonRootBean JsonRootBean: platformList){
            System.out.println(JsonRootBean);
        }
  }
```

**完成**

