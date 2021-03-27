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
<div class="title2" id="Mybatis-1">一、Mybatis入门</div>

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

    @Update("update comment set content = #{content},gmt_modified = now() where id = #{id}")
    int update(CommentDO commentDO);

    @Delete("delete from comment where id = #{key}")
    int delete(@Param("key")long id);
}
~~~

-----
<div class="title2" id="Mybatis-2">二、Mybatis XML</div>

-----
一般情况下，我们不使用注解而是用XML来执行SQL语句。

在application.properties中配置使用的映射xml文件。
~~~properties
mybatis.mapper-locations=classpath:[文件路径]/*.xml
~~~
配置完成并启动工程后，MyBatis会扫描指定路径，完成XML文件的加载。（如果有多个路径则用逗号隔开）

<div class="title3" id="Mybatis-2-1">2.1 Mybatis XML Mapper</div>

一个DAO.xml文件对应一个dao类。
其头信息为：
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
~~~

然后加上根节点mapper(表示映射哪个dao类)和子节点（resultMap,表示某DO类的各个属性和数据库属性的关联）
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="[DAO类的完整路径]">

  <resultMap id="[唯一标识]" type="[DO类（实体类）的完整路径]">
  <!-- id是设置主键 -->
    <id column="id" property="id"/>
    <!-- 其余属性 -->
    <result column="[数据库列名]" property="[DO类属性名]"/>
  </resultMap>

</mapper>
~~~
<div class="title4">具体语句映射</div>

~~~xml
    <insert id="[方法名]" parameterType="[DO类全名]" useGeneratedKeys="true" keyProperty="id">
        [对应insert的sql语句]
    </insert>

    <select id="[方法名]" resultMap="[resultMap id名]">
        [select 的sql语句]
    </select>
~~~
其他语句同理。（` useGeneratedKeys="true" keyProperty="id"`这俩属性是insert独有）
<div class="title3" id="Mybatis-2-2">2.2 XML模式开发顺序</div>



1. 创建DO对象
2. 创建DAO接口并配置`@Mapper`注解
3. 创建XML文件，完成`resultMap`配置
4. 创建DAO接口方法
5. 创建对应的XML语句

-----