---
title: JAVA数据库以及依赖等的配置
date: 2020-10-25 17:24:12
categories:
- Java Web
tags:
- dictionary
- MongoDB
- MySQL
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

<!-- ~~~
制表符
┏ ┳ ┓ ━ ╔ ╦ ╗ ═ ╒ ╤ ╕ 
┣ ╋ ┫ ┃ ╠ ╬ ╣ ║ ╞ ╪ ╡
┗ ┻ ┛   ╚ ╩ ╝   ╘ ╧ ╛

 ╱╲      ┄ ┄  ┅ ┅
╱╱╲╲
╲╲╱╱
 ╲╱      ┆ ┆  ┇ ┇ 
~~~ -->

<div class="name">by pawncs</div>

-----
<div class="title2">一、数据库</div>

-----
<div class="title3">1.1 MYSQL</div>

链接SQL：(YML)
~~~properties
spring.datasource.driver-class-name=……cj.……
spring.datasource.url=jdbc:mysql://[地址]:[端口号]/database_name
# database_name为数据库名
spring.datasource.username=[用户名]
spring.datasource.password=[密码]
# 上面是mysql的配置
~~~
JPA
~~~yml
jpa:
hibernate:
ddl-auto:create(或update)
show-sql:true
~~~
Spring DATA MYSQL配置(XML)  
	先加依赖。  `spring-boot-starter-data-jpa
mysql-connector-java`
~~~xml
<!--mybatis配置(XML)-->
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis</artifactId>
   <version>3.5.5</version>
</dependency>
~~~

<div class="title3">1.2 MongoDB</div>

连接MongoDB(proerties)
~~~properties
# 购买的云服务器的公网 IP
spring.data.mongodb.host=xxx.xxx.xxx.xxx
# MongoDB 服务的端口号
spring.data.mongodb.port=27017
# 创建的数据库及用户名和密码
spring.data.mongodb.database=practice
spring.data.mongodb.username=pppp
spring.data.mongodb.password=111aaa
~~~
spring data mongoDB 配置（XML）
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
~~~
-----
<div class="title2">二、其他依赖</div>

-----
Thymeleaf:(XML)  
添加 Thymeleaf 框架，打开 pom.xml 文件，添加以下的依赖，  
	添加后选 择 maven 的 auto import 
~~~xml
	<dependency> 
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-thymeleaf</artifactId> 
	</dependency> 
	<dependency>
		 <groupId>org.springframework.boot</groupId> 
		<artifactId>spring-boot-devtools</artifactId>
	 </dependency>
~~~



spring使用AOP(XML)  
使用AOP，添加依赖  
`spring-boot-starter-aop`(不加版本号)

spring session支持（XML）
~~~xml
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-core</artifactId>
</dependency>
~~~
Netty框架（XML）？？
~~~xml
<dependency>
 <groupId>io.netty</groupId>
 <artifactId>netty-all</artifactId>
 <version>4.1.38.Final</version>
</dependency>
~~~

Okhttp3库（XML）
~~~xml
<!-- https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp -->
<dependency>
 <groupId>com.squareup.okhttp3</groupId>
 <artifactId>okhttp</artifactId>
 <version>4.1.0</version>
</dependency>
~~~
fastjson依赖（XML）
~~~xml
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
~~~

easyexcel依赖（XML）
~~~xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>easyexcel</artifactId>
  <version>2.1.6</version>
</dependency>
~~~
lombok依赖（XML）
~~~xml
<dependency>  
          <groupId>org.projectlombok</groupId>  
          <artifactId>lombok</artifactId>   
</dependency>
~~~
JavaMail依赖 （XML）
~~~xml
<dependency>
 	<groupId>javax.mail</groupId>
<artifactId>mail</artifactId>
 	<version>1.4</version>
</dependency>
~~~
Kumo依赖（XML）（词云）
~~~xml
<dependency>
  <groupId>com.kennycason</groupId>
  <artifactId>kumo-core</artifactId>
  <version>1.17</version>
</dependency>
<!-- 下面tokenizers是为了中文分词引入 -->
<dependency>
  <groupId>com.kennycason</groupId>
  <artifactId>kumo-tokenizers</artifactId>
  <version>1.17</version>
</dependency>
~~~