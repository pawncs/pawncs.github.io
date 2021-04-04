---
title: Redis
date: 2021-04-01 20:40:22
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

-----
<div class="title2" id="Redis-1">一、Redis 简介</div>

-----
+ NoSQL
+ 基于内存运行
+ 支持分布式
+ 数据类型丰富（String,List,Set,Ordered Set）
<div class="title3" id="Redis-1-1">1.1、Redis 安装</div>

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

<div class="title3" id="Redis-1-2">1.2、Spring Boot 集成 Redis </div>

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

-----