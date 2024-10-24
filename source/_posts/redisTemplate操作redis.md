---
title: redisTemplate操作redis
date: 2022-06-30 16:47:59
author: Evan
categories: 笔记
tags:
---



# redisTemplate操作redis

**1.String操作**

```java
//String传入redis 
redisTemplate.opsForValue().set(key, value);

//从redis获取 
Object value = redisTemplate.opsForValue().get(key);

//设置过期时间
long time =60;
redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
```

**2.key操作**

```java
//判断key是否存在
Boolean exist =redisTemplate.hasKey(key);

//设置key过期时间
long time = 60;
redisTemplate.expire(key, time, TimeUnit.SECONDS);

//获取key过期时间
Long expire = redisTemplate.getExpire(key, TimeUnit.SECONDS);

// 删除key
redisTemplate.delete(key);
```

**3.散列操作**

```java
// 放入一个 hash ( key value ) item是hash的key
redisTemplate.opsForHash().put(key, item, value);

// 向hash中存放一个map
redisTemplate.opsForHash().putAll(key, map);

// 获取一个hash 的 所有key-value
Map<Object, Object> entries = redisTemplate.opsForHash().entries(key);

 // 获取一个hash 的 指定key 的value
Object value = redisTemplate.opsForHash().get(key, item);

// 删除指定 hash key 的value
redisTemplate.opsForHash().delete(key, item);

 // 是否存在 指定 hash 的key
Boolean exist = redisTemplate.opsForHash().hasKey(key, item);
```

**4.列表操作**

```java
// 列表右推入
redisTemplate.opsForList().rightPush(key, value);

// 列表左推入
redisTemplate.opsForList().leftPush(key, value);

// 列表左弹出
Object value = redisTemplate.opsForList().leftPop(key);

// 列表右弹出
Object value = redisTemplate.opsForList().rightPop(key);

// 将list右推入列表
redisTemplate.opsForList().rightPushAll(key, list);

// 修改列表指定索引的值
redisTemplate.opsForList().set(key, index, value);
 
// 获取列表指定索引的值
Object value = redisTemplate.opsForList().index(key, index);
```

**5.set 操作**

```java
// set 中存储值
redisTemplate.opsForSet().add(key, value1, value2);

// 从 set 中取值
Set<Object> members = redisTemplate.opsForSet().members(key);

// 判定 set 中是否存在 key-value
 Boolean member = redisTemplate.opsForSet().isMember(key, value);

//查询set长度
redisTemplate.opsForSet().size(key);

//从set中移除某些个元素
redisTemplate.opsForSet().remove(key,values);

//移除整个set
redisTemplate.delete(key);
```

**6.zset操作**

```java
//往Zset中添加元素
redisTemplate.opsForZSet().add(key, value, score);

//根据索引获取Zset的元素
redisTemplate.opsForZSet().range(key,begin,end);

//获取Zset的所有元素
getZset(key,0,-1);

// 往Zset中添加多个元素
// DefaultTypedTuple<String> p1 = new DefaultTypedTuple<>("value1", 1.0);
HashSet<DefaultTypedTuple<String>> defaultTypedTuples = new HashSet<>(list);
redisTemplate.opsForZSet().add(key,defaultTypedTuples);

//获取Zset中指定元素的分数
redisTemplate.opsForZSet().score(key,value);
    
//获取Zset集合的大小
redisTemplate.opsForZSet().size(key);
    
//获取Zset指定分数内集合的大小
redisTemplate.opsForZSet().count(key,begin,end);
    
//获取Zset指定分数内的集合
redisTemplate.opsForZSet().rangeByScore(key,begin,end);
    
//根据 Index 获取Zset集合 (包括分数)
redisTemplate.opsForZSet().rangeWithScores(key,begin, end);
    
//获取Zset集合
getZsetWithScore(key,0L, -1L);

//获取指定成员的排名   从大到小排   a,b,c,d     a排0
redisTemplate.opsForZSet().rank(key,value);

//获取指定成员的排名   从大到小排   a,b,c,d     a排3
redisTemplate.opsForZSet().reverseRank(key,value);
 
//从集合中删除指定元素
redisTemplate.opsForZSet().remove(key,value);
    
//从集合中删除指定范围索引元素
redisTemplate.opsForZSet().removeRange(key,begin,end);
  
//从集合中删除指定范围分数元素
redisTemplate.opsForZSet().removeRangeByScore(key,begin,end);
   
//修改指定元素的分数
redisTemplate.opsForZSet().incrementScore(key,value,score);
 
```

