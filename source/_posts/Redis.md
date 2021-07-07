---
title: Redis
date: 2021-04-23 20:40:22
categories:
- Java Web
tags:
- Redis
- dictionary
---
<style>
.title1{
    font-size:36px;
    color:#e7767f;
    /* 桃红 */

}
.title2{
    font-size:29px;
    color:#176f58;
    /* 祖母绿 */
}
.title3{
    font-size:22px;
    color:#21a675;
    /* 石绿 */
}
.title4{
    font-size:15px;
    color:#a8cd34;
    /* 柳绿 */
}
.name{

    margin-left: auto;
    text-align: right;
    color: #d05667;
    margin-right: 10px;
    margin-top: 20px;
    /*海棠红*/
}
</style>


<div class="name">by pawncs</div>

-----
<div class="title2">零、简介</div>

-----
<a href="#Redis-1">一、Redis简介</a>  
<a href="#Redis-1-1">1.1 nosql介绍</a>  
<a href="#Redis-1-2">1.2 redis介绍</a>  
<a href="#Redis-1-3">1.3 redis安装</a>  
<a href="#Redis-1-4">1.4 jedis介绍</a>  
<a href="#Redis-1-5">1.5 spring boot 集成 redis</a>  

<a href="#Redis-2">二、Redis常用数据类型</a>  
<a href="#Redis-2-1">2.1 String</a>  
<a href="#Redis-2-2">2.2 List</a>  
<a href="#Redis-2-3">2.3 Hash</a>  
<a href="#Redis-2-4">2.4 Set</a>  
<a href="#Redis-2-5">2.5 Sorted Set</a>  


<a href="#Redis-3">三、Redis key操作</a>  

<a href="#Redis-4">四、Redis 特性</a>  
4.1 多数据库  
4.2 支持事务

<a href="#Redis-5">五、Redis 持久化</a>  
<a href="#Redis-5-1">5.1 RDB(redis database)</a>  
<a href="#Redis-5-2">5.2 AOF(append only file)</a>  

<a href="#Redis-EX">EX、参考</a>  


-----
<div class="title2" id="Redis-1">一、Redis 简介</div>

-----
<div class="title3" id="Redis-1-1">1.1 nosql介绍</div>
NoSQL概述
+ 高并发读写
+ 海量数据的高效率存储和访问
+ 搞可扩展性和高可用性

NoSQL分类
+ 键值对存储（优势：快速查询）（劣势：缺少结构化）
+ 列存储（优势：查找速度快，扩展性强）（劣势：功能局限）
+ 文档数据库（优势：数据类型要求不严格）（劣势：性能不高，缺少统一查询语法）
+ 图形数据库（应用：社交网络啥的）（优势：应用图结构的算法）（劣势：需要对图进行运算才能得到结果，不方便做分布式集群方案）

特点：
+ 易扩展
+ 灵活的数据模型
+ 大数据量，高性能
+ 高可用
+ WEB2.0应用广泛

<div class="title3" id="Redis-1-2">1.2 redis介绍</div>
Redis:  
+ NoSQL
+ 基于内存运行
+ 支持分布式
+ 数据类型丰富（String,List,Set,Ordered Set）
+ 应用场景：缓存，任务队列，网站访问统计，数据过期处理，分布式中session的分离等


<div class="title3" id="Redis-1-3">1.3 Redis 安装</div>

安装Docker
~~~shell
sudo yum -y update
sudo yum -y install epel-release
sudo yum -y install docker-io
~~~
启动Docker
~~~shell
sudo systemctl start docker
sudo docker version
~~~

安装Redis并启动
~~~shell
sudo docker pull redis:latest
sudo docker images
sudo docker run  --name redis -p 6379:6379 -d --restart=always redis:latest redis-server --appendonly yes --requirepass "[密码]"
~~~
>6379是服务端口号
验证是否启动成功
~~~shell
sudo docker ps
~~~

<div class="title3" id="Redis-1-4">1.4 Jedis入门</div>

Jedis是Redis官方首选java客户端开发包

new Jedis("IP地址",端口号)

jedis.set,jedis.get  
连接池：  
~~~java
JedisPoolConfig config = new JedisPoolConfig();//连接池配置对象

config.setMaxTotal(30);//最大连接数

config.setMaxIdle(10);//最大空闲连接数

//获得连接池
JedisPool jedisPool = new JedisPool(config,[IP地址],[端口]);

//获得核心对象(标准点要try,catch,finnally)
Jedis jedis = jedisPool.getResource();
~~~

<div class="title3" id="Redis-1-5">1.5 Spring Boot 集成 Redis </div>

+ 引入依赖
+ 配置Redis服务器
+ 启动应用
  
依赖
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~
配置
~~~properties
# Redis服务器地址
spring.redis.host=
# Redis 服务器端口号
spring.redis.port=6379
# Redis 服务器密码 
spring.redis.password=
~~~

而后会自动配置`StringRedisTemplate`类的对象。

-----
-----
<div class="title2" id="Redis-2">二、Redis常用数据类型</div>

-----
+ String(字符串)
+ list(字符串列表)
+ hash（哈希）
+ set（字符串集合）
+ sorted set（有序字符串集合）

key不要太长（降低性能），也不要太短（降低可读性），要有统一命名规范。

<div class="title3" id="Redis-2-1">2.1 String</div>

+ 在redis中以2进制操作,安全 
+ value 最常数据为:512M
+ 常用命令：赋值，取值，删除，数值增减，扩展命令。
+ get set getset del incr decr incrby append
<div class="title3" id="Redis-2-2">2.2 Hash</div>

+ <strong>String</strong> key 和<strong>String</strong> value的map容器
+ 可以存储4294967295个键值对（4*10^9）
+ 常用命令(myhash是key,Hash是value)
  + 赋值 hset myhash key value ;hmset(存多个)
  + 取值 hget myhash key;hmget(取多个)；hgetall （同时获得键值对）
  + 删除 hdel myhash key1 key2
  + 增加数字 hincrby myhash key
  + 扩展命令 (判断指定属性是否存在) hexists …… (获得属性数量) hlen myhash (获得所有key) hkeys myhash

<div class="title3" id="Redis-2-3">2.3 List</div>

+ 可以在左侧和右侧添加链表
+ ArrayList使用数组方式
+ linkedList使用双向链表方式
+ 双向链表添加数据
+ 双向链表删除数据
+ 常用命令
  + 两端添加  
     lpush key a b c(a在最后面，c在最左侧)；rpush ……（从右端添加）
  + 查看列表  
    lrange key start end(0表示第一个，-1表示最后一个)
  + 两端弹出  
    lpop key(弹出并返回)
  + 获取列表元素个数  
    llen key
  + 扩展命令  
    lpushx  key (仅当key存在才插入)；  
    lrem key count value(count个为value的元素，总共删除count个。其为0时全删)；  
    lset key index value(在角标为index的位置添加value);  
    linsert key after value1 value2(在value1后添加value2)
+ 使用场景：
  经常用于消息队列的服务，来<strong>完成多个程序的消息的交互</strong>（存消息的是生产者，取的是消费者）——>消费者可以在处理时添加进备份队列，处理完后从备份队列中删除，防止程序崩溃导致的消息丢失。备份中消息过期时可以重新添加进主队列中
<div class="title3" id="Redis-2-4">2.4 Set</div>

+ 没有排序的字符集合
+ 和list不同：不允许出现重复元素
+ 可以在服务端完成多个set的聚合操作(节省网络开销)
+  最大元素数量：4294967295
+  常用命令
     + 添加、删除 sadd key value1 value2 ;srem ……
     + 获得元素 sismember key value(返回1表存在 0表不存在) smembers key(获得所有元素)
     + 差集运算 sdiff key1 key2
     + 交集运算 sinter key1 key2
     + 并集运算 sunion ……
     + 扩展命令 srandmember ;scard; sdiffstore keyTarget key1 key2(将key1和key2差集运算的结果存储到keyTarget)(其他类似)
+ 使用场景：用于跟踪一些具有唯一性的数据（比如访问博客的IP地址）（可以利用服务器端的运算来操作所有购买者的ID存储到set）
<div class="title3" id="Redis-2-5">2.5 Sorted-Set</div>

+ 和set的区别：sorted-set中的每个元素都有一个分数与之关联。redis通过这个分数将其从小到大排序。元素唯一，分数可以重复。时间复杂度为集合中元素的对数。因为其有序，所以访问中部的成员也是高效的。
+ 对于1来说，想要在其他数据库中达到同样的效果是非常困难的。
+ 应用场景：游戏排名，微博热点之类
+ 常用命令
  + 添加元素 zadd key score value score value(每个元素都要有个分数，返回的是添加的个数)（若已有元素，则更改分数，不算进添加的个数）
  + 获得元素 zscore key value(获得value的分数)； zcard key(返回数量)
  + 删除元素 zrem key value1 value2(返回删除的个数)
  + 范围查询 zrange key start end(0是第一个，-1是最后一个。返回查询到的所有元素)；  
    zrange key start end withscores(带分数查询)  
    zrevrange …… (降序排序)
    zremrangebyrank key start end（范围删除）
            zremrangebyscore ……（根据分数范围进行删除）
  + 扩展命令   
    zrangebyscore ……（返回分数在某个区间的元素） limit 2(只显示2个)；  
    zincrby key score value(将value的分数加上score)  
    zcount key 80 90(统计分数位于80-90区间的个数)
+  使用场景 积分排行榜（zadd更新分数，zrange获得积分）构建索引数据

----
----
<div class="title2" id="Redis-3">三、Redis key 操作</div>

----

1. 查看所有keys ： keys *
2. 查看my开头的所有keys : keys my?
3. 删除指定key: del my1 my2 my3
4. 查看指定是否存在：exists my1
5. 重命名：rename key1 newName
6. 设置过期时间：expire key time(time的单位是s)
7. 查看超时时间 ttl key(key所剩时间,没设置返回-1)
8. 查看key类型：type key

----
----
<div class="title2" id="Redis-4">四、Redis 特性</div>

----

+ 多数据库
  + 一个redis实例可以包括多个数据库，客户端可以指定连接哪个数据库。一个redis实例最多可提供16个数据库，下标为0-15，客户端默认连接0号数据库，可通过select选择具体连接哪个数据库
    select 1  
  + 移动key去其他数据库：move key 1(将key移动到1号数据库)
+ 支持事务
  + multi exec discard
  + 串行化顺序执行。在事务执行的过程中，redis不会向其他客户端提供任何服务，保证所有命令原子化执行。在redis中，某个命令执行失败，后面的命令仍然被执行。
  + multi 开启一个事务。之后的命令都是事务的命令。这些命令都会存储到队列当中。直到exec
  + exec 相当于提交事务
  + discard 相当于回滚事务

----
----
<div class="title2" id="Redis-5">五、Redis 持久化</div>

----

redis的高性能：所有数据都存储在内存当中。为了数据不丢失，所以要存储到硬盘上。  
两种持久化方式：
+ RDB方式
  + 默认支持
  + 在指定的时间间隔内，将内存的数据快照写进硬盘
+ AOF方式
  + 以日志的方式存储服务器的每一个操作。服务器启动之初，会用日志来重新构建数据库，保证数据是完整的
+ 不持久化
  + 通过配置禁用RDB的功能。此时redis就是缓存的机制了。
+ 同时使用RDB,AOF

<div class="title3" id="Redis-5-1">5.1 RDB(redis database)</div>

优势：
+ 只包含一个文件，利于文件备份。（每天归档一次，每30天归档一次……）当系统出现灾难性故障，可以非常容易的进行恢复。
+ 性能最大化。由（fork）子进程来完成持久化的工作。避免服务器进程执行IO操作。与AOF相比，若数据集很大，RDB启动效率更高。  
  
劣势：
+ 无法完全保证数据不丢失
+ 当数据集很大时，要服务器停止一段时间来完成快照的录入

配置：  
redis.conf中（100多行左右）
~~~
save 900 1
save 300 10
save 60 10000
~~~
意为：900s内有一个数据改动，300s内有10个数据改动，或60s内有10000个数据改动，则记录一次快照

~~~
dbfilename dump.rdb
~~~
意为数据的文件名

~~~
dir ./
~~~
为保存路径

<div class="title3" id="Redis-5-2">5.2 AOF(append only file)</div>

优势：
+ 更高的数据安全性
  + 三种同步策略：每秒同步（异步完成），每修改同步（同步持久化，效率最低，最安全），不同步
+ 对日志的操作是append模式（追加）。因此写入过程宕机不会影响已经写入的内容（一条只写入了一半数据时，启动之前，通过reids-check-aof 来解决数据一致性的问题）
+ 日志过大，则redis自动启动重写机制，使用append不断向老的磁盘写入数据，同时创建新的日志来记录此期间产生的数据。因此重写切换能更好的保证安全性
+ AOF包含一个格式清晰，易于理解的日志文件，用于记录所有的修改操作。实际上也可以通过这个文件来完成数据的重建

劣势：
+ 比RDB更大
+ 根据同步策略，效率往往低于RDB

配置  
还是redis.conf
~~~
appendonly no
~~~
默认情况下aof没有打开，需要改成yes
~~~
appendonlyfilename "appendonly.aof"
~~~
日志文件名
~~~
appendfsync always
#appendfsync everysec
#appendfsync no
~~~
同步策略，每修改同步，每秒同步，不同步

-----
-----
<div class="title2" id="Redis-EX">EX、参考</div>

-----

+ [Redis入门](https://www.imooc.com/learn/839)

-----