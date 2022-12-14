---
title: Redis笔记
date: 2021-11-15
tags: Redis
categories: 笔记
description: Redis笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/9.jpg
---
Redis是一个基于内存的key-value结构数据库

- 基于内存存储，读写性能高
- 适合存储热点数据(热点商品，咨询，新闻)
- 企业应用广泛

# 安装Redis

[下载地址](https://download.redis.io/releases/)

1. 将Redis安装包上传到Linux
2. 解压安装包：tar -zxvf redis-4.0.0.tar.gz -C /usr/local
3. 安装Redis的依赖环境gcc，命令：yum install gcc-c++
4. 进入/usr/local/redis-4.0.0，进行编译，命令：make
5. 进入redis的src目录，进行安装，命令：make install

**Redis服务启动与停止**

- 在redis/src中启动：./redis-server(ctrl + c停止运行)

- 复制一个shell窗口，在redis/src目录下启动客户端：./redis-cli

- 修改配置文件使得redis后台运行：

  在redis目录下：vim redis.conf

  /dae：快速查找daemonize no

  daemonize no改为daemonize yes

- 添加配置文件参数并启动：src/redis-server ./redis.conf

**设置密码**

- 在redis目录下：vim redis.conf

- /requirepass：快速查找

- 打开/requirepass的注释，后边跟的是密码

- 添加配置文件参数并启动：src/redis-server ./redis.conf

- 在redis/src目录下启动客户端：./redis-cli -h localhost -p 6379

  -h：IP地址     -p：端口号

  在127.0.0.1:6379>后输入：auth 123456(密码)

  或者./redis-cli -h localhost -p 6379 -a 123456

**远程连接**

- 在配置文件中开启远程连接：vim redis.conf

  /bind：找到bind 127.0.0.1，注释掉

- 开放6379端口：firewall-cmd --zone=public --add-port=6379/tcp --permanent

  立即生效：firewall-cmd --reload

- 在windows的redis目录打开powershell(shift + 右键)

- 输入命令：./redis-cli.exe -h 107.182.19.176 -p 6379

  接着输入：auth 123456

# Redis数据类型

**Redis数据类型**：Redis存储的是key-value结构的数据，key是字符串类型，value有五种用的数据类型

- 字符串string
- 哈希hash
- 列表list
- 无序集合set
- 有序集合sorted set

## **Redis中字符串String常用命令**

- set key value：设置指定key的值（会覆盖之前设置的值）
- get key：获取指定key的值
- setex key seconds value：设置指定key的值，并将key的过期时间设为seconds秒
- setnx key value：只有在key不存在时设置key的值

## **Redis中hash操作命令**

- HSET key field value：将哈希表中key中的字段field的值设为value

  例如：hset 001 name xiaoming

  hset 001 age 10

- HGET key field：获取存储在哈希表中指定字段的值

- HDEL key field：删除存储在哈希表中的指定字段

- HKEYS key：获取哈希表中所有字段

- HVALS key：湖片区哈希表中所有值

- HGETALL key：获取在哈希表中指定key的所有字段和值

## **Redis中列表list操作命令**

- LPUSH key value1 [value2]：将一个或多个值插入到列表头部（元素的值可以重复）

  例：lpush mylist a b c

- lrange key start stop：获取列表指定范围内的元素

  例：lpush mylist 0 -1：获取从开始到结尾所有元素

- rpop key：移除并获取列表最后一个元素

- llen key：获取列表长度

- brpop key1 [key2] timeout：移出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止

## **Redis中无序集合set操作命令**

- sadd key member1 [member2]：向集合添加一个或多个成员（元素的值不能重复）

- smembers key：返回集合中的所有成员

- scard key：获取集合的成员数

- sinter key1 [key2]：返回给定所有集合的交集

- sunion key1 [key2]：返回所有给定集合的并集

- sdiff key1 [key2]：返回给定所有集合的差集

  如key1：1 2 3 4    key2： a b 1 2   返回结果为：3  4，互换位置后返货结果为：a b

- srem key member1 [member2]：一处集合中一个或多个成员

## **Redis中有序集合sorted set操作命令**

Redis sorted set有序集合是string类型元素的集合，且不允许重复的成员。每个元素都会关联一个double类型的分数：score。redis正是通过分数来为集合中的成员进行从小到大的排序。有序集合的成员是唯一的，但分数可以重复

- ZADD key score1 member1 [score2 member2]：向有序集合添加一个或多个成员，或者更新已存在成员的分数
- ZRANGE key start stop [WITHSCORES]：通过索引区间返回有序集合中指定区间内的成员（按分数从小到大），加上withcores可以同时返回分数
- ZINCRBY key increment member：有序集合中指定成员的分数加上增量increment
- zrem key member [member ...]：移除有序集合中的一个或多个成员

## **Redis通用命令**

- keys pattern：查找所有给定模式的key

- exists key：检查给定key是否存在
- type key：返回key所存储的值的类型
- ttl key：返回给定key的剩余生存时间，以秒为单位
- del key：当key存在时删除key
- select 数字：切换数据库，redis默认提供16个数据库，默认使用0号数据库

# 在Java中操作redis

介绍

Redis的Java客户端很多，官方推荐的有三种

- Jedis
- Lettuce
- Redisson

Spring对Redis客户端进行了整合，提供了Spring Data Redis，在Spring Boot项目中还提供了对应的Starter，即Spring-boot-starter-data-redis

## Jedis

- 导入坐标

  ```xml
  <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.8.0</version>
  </dependency>
  ```

- ``` java
  @Test
  public void testRedis(){
      //1.获取连接
      Jedis jedis = new Jedis("107.182.19.176",6379);
      jedis.auth("123456");
  
      //2.执行具体的操作
      jedis.set("name","小明");
  
      //3.关闭连接
      jedis.close();
  }
  ```

## Spring data Redis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```yml
spring:
 redis:
  host: 107.182.19.176
  port: 6379
  password: 123456
  database: 0 #Redis默认提供16个数据库，默认使用0号数据库
  jedis:
    #Redis连接池配置
    pool:
      max-active: 8 #最大连接数设置
      max-wait: 1ms #连接池最大阻塞等待时间
      max-idle: 4 #连接池中的最大空闲连接，最好和max-active设置的值相同
      min-idle: 0 #连接池中的最小空闲连接
```

Spring Data Redis中提供了一个高度封装的类：RedisTemplate，针对jedis客户端中大量的api进行了归类封装，将同一类型操作封装为operation接口

- ValueOperations：简单K-V操作（string）
- HashOperations：针对hash类型的数据操作
- ListOperations：针对list类型的数据操作
- SetOperations：无序集合set类型数据操作
- ZSetOperations：针对map类型的数据操作（有序集合）

### **设置Redis配置类**

```java
/**
 * "\xac\xed\x00\x05t\x00\x03age"默认序列化器
 * 设置新的序列化器后显示为age
 * value不需要新的序列化器，因为反序列化后会恢复
 * 即使自己不创建RedisTemplate，框架也会创建
 * 自己创建是为了修改key序列化器，创建后框架不再创建
 */
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    //自动装配连接工厂
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory) {

        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();

        //默认的Key序列化器为：JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());//设置新的序列化器
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());//设置hash的key的序列化器

        redisTemplate.setConnectionFactory(connectionFactory);//设置连接工厂
        return redisTemplate;
    }
}
```

### 操作String类型数据

redisTemplate.opsForValue()：操作String类型数据

- set key value：设置指定key的值（会覆盖之前设置的值）

  ```java
  redisTemplate.opsForValue().set("xxx","xiaoming");
  ```

- get key：获取指定key的值

  ```java
  String xxx = (String) redisTemplate.opsForValue().get("xxx");
  ```

- setex key seconds value：设置指定key的值，并将key的过期时间设为seconds秒

  ```java
  //10l:long型的10，过期时间，单位为秒
  redisTemplate.opsForValue().set("sex","man",10l, TimeUnit.SECONDS);
  ```

- setnx key value：只有在key不存在时设置key的值

  ```java
  Boolean aBoolean = redisTemplate.opsForValue().setIfAbsent("city", "henan");
  ```

### 操作hash类型数据

```java
HashOperations hashOperations = redisTemplate.opsForHash();
```

- HSET key field value：将哈希表中key中的字段field的值设为value

  例如：hset 001 name xiaoming

  hset 001 age 10

  ```java
  hashOperations.put("001","name","xiaohong");
  hashOperations.put("001","age","10");
  ```

- HGET key field：获取存储在哈希表中指定字段的值

  ```java
  String age = (String) hashOperations.get("001", "age");
  ```

- HDEL key field：删除存储在哈希表中的指定字段

  ```java
  hashOperations.delete("001","name");
  ```

- HKEYS key：获取哈希表中所有字段

  ```java
  Set<String> keys = hashOperations.keys("001");
  ```

- HVALS key：湖片区哈希表中所有值

  ```java
  List values = hashOperations.values("001");
  ```

- HGETALL key：获取在哈希表中指定key的所有字段和值

### 操作list类型数据

```java
ListOperations listOperations = redisTemplate.opsForList();
```

- LPUSH key value1 [value2]：将一个或多个值插入到列表头部（元素的值可以重复）

  例：lpush mylist a b c

  ```java
  listOperations.leftPush("mylist","1");//插入头部
  listOperations.leftPushAll("mylist1","1","2","3");//批量插入头部
  listOperations.rightPushAll("mylist1","1","2","3");//批量插入尾部
  listOperations.rightPush("mylist","5");//插入尾部
  ```

- lrange key start stop：获取列表指定范围内的元素

  例：lrange mylist 0 -1：获取从开始到结尾所有元素

  ```java
  listOperations.range("mylist",0,-1);//获取从开始到结尾所有元素
  listOperations.range("mylist",1,5);//获取头部第二个到第六个元素
  ```

- rpop key：移除并获取列表最后一个元素

  ```java
  Object mylist1 = listOperations.rightPop("mylist");//移除并获取列表最后一个元素
  Object mylist2 = listOperations.leftPop("mylist");//移除并获取列表第一个元素
  ```

- llen key：获取列表长度

  ```java
  Long size = listOperations.size("mylist");
  ```

- brpop key1 [key2] timeout：移出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止

  ```java
  listOperations.rightPop("mylist",10,TimeUnit.SECONDS);//移出并获取列表的最后一个元素
  ```

### 操作set类型数据

- sadd key member1 [member2]：向集合添加一个或多个成员（元素的值不能重复）

  ```java
  setOperations.add("myset","1","2","3","1");//不能重复
  ```

- smembers key：返回集合中的所有成员

  ```java
  Set<String> myset = setOperations.members("myset");
  ```

- scard key：获取集合的成员数

  ```java
  Long size = setOperations.size("myset");
  ```

- sinter key1 [key2]：返回给定所有集合的交集

  ```java
  Set<String> intersect = setOperations.intersect("myset", "myset1");
  ```

- sunion key1 [key2]：返回所有给定集合的并集

  ```java
  Set<String> union = setOperations.union("myset", "myset1");
  Set<String> union = setOperations.union(list);//list：key的集合
  ```

- sdiff key1 [key2]：返回给定所有集合的差集

  如key1：1 2 3 4    key2： a b 1 2   返回结果为：3  4，互换位置后返货结果为：a b

  ```java
  Set<String> difference = setOperations.difference("myset", "myset1");
  ```

- srem key member1 [member2]：移除集合中一个或多个成员

  ```java
  setOperations.remove("myset","1","2");
  ```

### 操作有序集合zset数据

- ZADD key score1 member1 [score2 member2]：向有序集合添加一个或多个成员，或者更新已存在成员的分数

  ```java
  zSetOperations.add("myZset","a",10.0);
  ```

- ZRANGE key start stop [WITHSCORES]：通过索引区间返回有序集合中指定区间内的成员（按分数从小到大），加上withcores可以同时返回分数

  ```java
  Set<String> myZset = zSetOperations.range("myZset", 0, -1);//默认分数从小到大
  Set<String> myZset1 = zSetOperations.rangeWithScores("myZset", 0, -1);//同时返回分数
  ```

- ZINCRBY key increment member：有序集合中指定成员的分数加上增量increment

  ```java
  zSetOperations.incrementScore("myZset","c",100);
  ```

- zrem key member [member ...]：移除有序集合中的一个或多个成员

  ```java
  zSetOperations.remove("myZset","a","b");
  ```

### 通用操作

- keys pattern：查找所有给定模式的key

  keys *：获取所有key

  ```java
  redisTemplate.keys("*");
  ```

- exists key：检查给定key是否存在

  ```java
  redisTemplate.hasKey("001");
  ```

- type key：返回key所存储的值的类型

  ```java
  DataType dataType = redisTemplate.type("myset");
  ```

- ttl key：返回给定key的剩余生存时间，以秒为单位

  ```java
  redisTemplate.getExpire("sex");
  ```

- del key：当key存在时删除key

  ```java
  redisTemplate.delete("001");
  ```

- select 数字：切换数据库，redis默认提供16个数据库，默认使用0号数据库