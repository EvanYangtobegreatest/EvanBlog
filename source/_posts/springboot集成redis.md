---
title: springboot集成redis
date: 2022-06-30 14:20:32
author: Evan
categories: 笔记
tags: 
---

# springboot 集成redis

## redis是什么

Redis 是一种非关系型数据库（NoSQL），NoSQL 是以 key-value 的形式存储的，和传统的关系型数据库不一样，不一定遵循传统数据库的一些基本要求，比如说 SQL 标准，ACID 属性，表结构等等，这类数据库主要有以下特点：非关系型的、分布式的、开源的、水平可扩展的。NoSQL 使用场景有：对数据高并发读写、对海量数据的高效率存储和访问、对数据的高可扩展性和高可用性等等。

## redis数据类型

**String（字符串）**

string 是 redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。

string 类型是二进制安全的。意思是 redis 的 string 可以包含任何数据。比如jpg图片或者序列化的对象。

string 类型是 Redis 最基本的数据类型，string 类型的值最大能存储 512MB。

**Hash（哈希）**

Redis hash 是一个键值(key=>value)对集合。

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。

**List（列表）**

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

**Set（集合）**

Redis 的 Set 是 string 类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

**zset（sorted set：有序集合）**

Redis  zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

## Springboot如何集成使用redis

**1.添加依赖**

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
 </dependency>
```

**2.创建Redis的Config类**

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        // 创建redisTemplate
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(connectionFactory);

        // 使用Jackson2JsonRedisSerialize替换默认序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new 	 Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        // key采用String的序列化方式
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // value序列化方式采用jackson
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的key也采用String的序列化方式
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        // hash的value序列化方式采用jackson
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}
```

**3.启动类上添加@EnableCaching注解**

```java
@SpringBootApplication()
@EnableCaching
public class MyspringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyspringbootApplication.class, args);
    }
}
```

到这里就完成springboot集成redis了。

## redis数据操作

[点击查看redis在springboot的操作](http://yangyewen.xyz/2022/06/30/redisTemplate%E6%93%8D%E4%BD%9Credis/)

## 利用redis实现缓存功能

原理很简单，先在redis中查询是否有数据，若没有就去数据库查询。

如果redis有，就返回redis中的数据。

```java
 @Autowired
    RedisTemplate<String,Object> redisTemplate;

@ResponseBody
    @RequestMapping(value = "/api/user") //查询数据
    public List<User> getuser() {
        List<User> user;
        //先在redis中查找
        user=( List<User>)redisTemplate.opsForValue().get("user");
        //如果redis没有就查询数据库
         if(user==null){
             user=userrService.queryuser();
         }
        //将查到的数据放入redis缓存中
        redisTemplate.opsForValue().set("user", user);
        return user;
    }
```

