---
title: MyBatis
date: 2021-03-29 08:57:24
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
<a class="title4" href="#Mybatis-1">一、MyBatis 入门</a>  
<a href="#Mybatis-1-1">1.1 简介</a>   
<a href="#Mybatis-1-2">1.2 sql语句</a>  
<a href="#Mybatis-1-3">1.3  MyBatis 映射对象</a>  
<a href="#Mybatis-1-4">1.4 MyBatis DAO</a>  
<a  class="title4" href="#Mybatis-2">二、MyBatis XML</a>  
<a href="#Mybatis-2-1">2.1 Mybatis XML Mapper</a>   
<a href="#Mybatis-2-2">2.2 XML模式开发顺序</a>  
<a class="title4" href="#Mybatis-3">三、MyBatis 动态SQL</a>  
<a href="#Mybatis-3-1">3.1 条件</a>   
<a href="#Mybatis-3-2">3.2 循环</a>  
<a class="title4" href="#Mybatis-4">四、MyBatis 分页</a>  
<a class="title4" href="#Mybatis-ex">EX、MyBatis 开发文档</a>  

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
<div class="title2" id="Mybatis-3">三、MyBatis 动态SQL</div>
<div class="title3" id="Mybatis-3-1">3.1 条件</div>

~~~xml
<select id="search" resultMap="userResultMap">
  select * from user where
    <if test="keyWord != null">
      user_name like CONCAT('%',#{keyWord},'%')
        or nick_name like CONCAT('%',#{keyWord},'%')
    </if>
    <if test="time != null">
      and  gmt_created <![CDATA[ >= ]]> #{time}
    </if>
</select>
~~~
这里test通过才会让语句生效。
> 为了防止batis解析失败，不能出现`><=%&`等符号。所以用` <![CDATA[ key ]]>`包裹。

这里if未生效就会产生错误语句，所以将`where`换成对应语句。
~~~xml
<select id="search" resultMap="userResultMap">
  select * from user
   <where>
      <if test="keyWord != null">
          user_name like CONCAT('%',#{keyWord},'%')
            or nick_name like CONCAT('%',#{keyWord},'%')
      </if>
      <if test="time != null">
        and  gmt_created <![CDATA[ >= ]]> #{time}
      </if>
   </where>
</select>
~~~

<div class="title3" id="Mybatis-3-2">3.2 循环</div>

foreach标签
~~~xml
<!-- 例1 -->
<insert id="[方法名]" parameterType="java.util.List" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
    VALUES
    <foreach collection="list" item="it" index="index" separator =",">
        (#{it.userName}, #{it.pwd}, #{it.nickName}, #{it.avatar},now(),now())
    </foreach >
</insert>
<!-- 例2 -->
<select id="findByIds" resultMap="userResultMap">
    select * from user
    <where>
        id in
        <foreach item="item" index="index" collection="ids"
                    open="(" separator="," close=")">
            #{item}
        </foreach>
    </where>
</select>
~~~
`separator`是给每条记录添加的分隔符  
`index`是集合的索引值
`open`和`close`是节点开始结束时自定义的分隔符

<div class="title2" id="Mybatis-4">四、MyBatis 分页</div>

-----
通过第三方插件`pagehelper`来完成分页功能。
>`pagehelper`的依赖

~~~xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.13</version>
</dependency>
~~~
`PageHelper.startPage(1, 3);`,第一个参数指定查询页数，第二个参数指定每页数目（从1开始而不是从0）。返回的是`Page`类型的对象。
~~~java
    public Paging<UserDO> getAll() {
        // 设置当前页数为1，以及每页3条记录
        Page<UserDO> page = PageHelper.startPage(1, 3).doSelectPage(() -> userDAO.findAll());

        return new Paging<>(page.getPageNum(), page.getPageSize(), page.getPages(), page.getTotal(), page
                .getResult());
    }
}
~~~
这里Paging是自制的分装类。

-----
<div class="title2" id="Mybatis-ex">EX、文档</div>

[mybatis开发文档](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)

-----

拓展.连接池,HikariCP(spring),[Druid(ali)](https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter)