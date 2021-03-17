---
title: MyBatis
date: 2021-03-17 08:57:24
categories:
- Java Web
tags:
- MyBatis
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
<div class="title2">零、目录</div>

-----
一、MyBatis 入门  
<a href="#Mybatis-1-2">1.2 sql语句</a> 

-----
<div class="title2">一、Mybatis入门</div>

-----
<div class="title3">1.1 简介</div>

  >Mybatis优势：可以灵活自定义SQL又兼具ORM框架的特性

+ ORM(Object Relational Mapping)：对象关系映射，用于实现面向对象编程语言里不同类型系统的数据之间的转换
+ 基于JDBC(JAVA领域所有数据库框架都基于JDBC) 

集成Mybatis  
1. 添加Spring Web依赖
2. 添加MyBatis Framework依赖
3. 添加MySQL Driver依赖
~~~xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
~~~
<p style="color:red">此时无法启动，因为找不到数据库连接</p>

~~~properties
spring.datasource.url=jdbc:mysql://mysql数据库地址:数据库端口/数据库名称?serverTimezone=GMT%2B8
spring.datasource.username=用户名
spring.datasource.password=密码
~~~
`serverTimezone=GMT%2B8` 的目的是设置时区为中国时区

<div class="title3" id="Mybatis-1-2">1.2 sql语句</div>

[sql语句](https://www.w3school.com.cn/sql/sql_create_table.asp)


<div class="title3" id="Mybatis-1-3">1.3 MyBatis 映射对象</div>

+ DO 对象(data object)：  
  用来映射数据库的表的对象。
  一般放在dataobject的包下
  + 注意：`DATETIME` 映射 `java.util.Date`

<div class="title3" id="Mybatis-1-4">1.4 MyBatis DAO</div>

+ DAO层 (data access object):DAO层会包含对数据库操作的接口和实现类

+ 增删改查
~~~java
@Mapper
public interface UserDAO {
    //这里用了别名解决数据库名称和DO类名称不一致的问题
    @Select("SELECT id,user_name as userName,pwd,nick_name as nickName,avatar,gmt_created as gmtCreated,gmt_modified as gmtModified FROM user")
    List<UserDO> findAll();

    //这里用了#{userName}来导入参数中的属性
     @Insert("INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified) VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())")
     //三个设置分别是：允许数据库使用自增主键、设置表的主键字段名称，设置DO模型的主键字段
    @Options(useGeneratedKeys = true, keyColumn = "id", keyProperty = "id")
 
  int insert(UserDO userDO);

}
~~~

-----