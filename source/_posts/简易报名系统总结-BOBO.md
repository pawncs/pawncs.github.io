---
title: 简易报名系统总结-BOBO
date: 2020-07-07 17:28:59
categories:
- Java Web
tags: 
- note
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
</style>

-----
<div class="title1">零、目录</div>

---
~~~
┌----- 一、创建Spring boot工程
|       ├-----1.1 创建
|       └-----1.2 让页面输出hello world
|
├----- 二、使用mybatis连接数据库
|       ├-----2.1 学习网址
|       ├-----2.2 配置
|       ├-----2.3 使用mybatis
|       └-----2.4 创建业务层
|
├----- 三、使用freemaker实现动态页面生成
|       ├-----3.1 freeMaker在线手册
|       ├-----3.2 配置
|       └-----3.3 使用freeMaker
|
└----- 四、附录
        ├-----4.1 css文件
        └-----4.2 打包并运行
~~~
---
<div class="title1">一、创建Spring boot工程</div>

---
<div class="title3">1.1 创建</div>

可以选择在IDEA内部直接创建，但我更喜欢使用[spring initializr](start.spring.io)的创建方式。选用Maven 和 JAVA11.
添加的依赖选择Spring Web.然后下载到本地解压。完成Spring boot工程的创建。
>这里没有选用 mybatis framework是因为这个依赖会导致在没有连接数据库时项目无法启动，所以在后面需要连接数据库时再添加。  
<div class="title3">1.2 让页面输出hello world</div>

src.main.java包内写源码，src.main.resources包内写配置、模板和其他一些静态资源等。

新建一个控制类：`DemoController`。添加注解`@RestController`使该类的方法只返回数据而不是返回页面。（或者可以在类上加注解@Controller，方法上添加注解`@Responsebody`）.类内写一个无参数的方法调用时返回"hello world"就完成了。
~~~java
   @GetMapping("/helloWorld")
    public String helloWorld(){
        return "helloWorld";
    }
~~~
运行主函数后访问localhost:8080/helloWorld即可。

---
<div class="title1">二、使用mybatis连接数据库</div>

---
<div class="title3">2.1 学习网址</div>

[视频地址](https://www.bilibili.com/video/BV1Db411s7F5)
<div class="title3">2.2 配置</div>

首先需要在pom.xml中添加相关的依赖。
~~~xml
        <dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.5</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
        </dependency>
~~~
需要一个实体类，其每一个属性都是数据库中的一个字段，用来做读取输入操作。
~~~java
package com.bobo.second.domain;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable {
    private Integer id;
    private Date gmt_create;
    private Date gmt_modified;
    private String name;
    private String gender;

   //省略了getter、setter和toString()方法。
}
~~~
>这里实现了空接口Serializable是为了标记该类，说明其是可序列化的。

需要一个持久层的接口，来做一切mysql的增删改查方法。并不需要具体实现，mybatis会做这件事。
~~~java
package com.bobo.second.dao;

import com.bobo.second.domain.User;
import java.util.List;

public interface UserDaoService {
    /**
     * 查询所有记录
     * @return
     */
    List<User> findAll();

    /**
     * 插入记录
     * @param user
     * @return
     */
    void insert(User user);

    /**
     * 根据Id返回记录
     * @param id
     * @return
     */
    User findById(Integer id);

    /**
     * 根据ID删除记录
     * @param id
     */
    void deleteById(Integer id);

    /**
     * 根据记录修改记录
     * @param user
     * @return
     */
    void update(User user);

    //寻得该表中最大的ID
    int findMaxId();
}
~~~

然后需要一个主配置文件，内含连接数据库的信息，接着需要一个映射配置文件，使得sql语句可以和dao接口对应起来。（这里也可以在dao接口内直接用注解书写对应的sql语句而不使用映射配置文件，但是为了让sql语句和java代码分离，还是选择了xml的方式）
>所有配置信息都在`resources`中
~~~xml
<!--主配置文件mybatisConfig.xml-->
<!-- 此处为约束，复制粘贴得 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 此处为具体的配置信息 -->
<configuration>
    <environments default="mysql">
        <!-- 该处环境配置id必须和之前defult的内容相同 -->
        <environment id="mysql">
            <!--配置事务类型，这里为“JDBC”-->
            <transactionManager type="JDBC"/>
            <!-- 配置连接池（连接数据库有关的信息） -->
            <dataSource type="POOLED">
                <!-- 驱动，老版应该是没有cj的 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!-- 数据库地址，jdbc:mysql://是固定的，后面是数据库地址，如果是本地就是localhost,后跟端口号和数据库名。如果无法连接再加上后面的字符集等信息。 -->
                <!-- 此处用"&amp;"代替了"&" -->
                <property name="url" value="jdbc:mysql://[数据库地址]:[端口号]/[数据库名]?useUnicode=true&amp;zeroDateTimeBehavior=convertToNull&amp;autoReconnect=true"/>
                <property name="username" value="[用户名]"/>
                <property name="password" value="[密码]"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!-- 将映射配置文件的位置导入主配置文件中 -->
        <!-- 如果采取注解的方式，此处应导入dao接口的class -->
        <mapper resource="com/bobo/second/dao/UserDaoService.xml"/>
        
    </mappers>
</configuration>
~~~
~~~xml
<!-- 映射配置文件UserDaoService.xml -->
<!-- 此处为约束，复制粘贴得 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 映射的名字空间注意和映射的接口的全限定类名相同 -->
<mapper namespace="com.bobo.second.dao.UserDaoService">
    <!-- SQL语句 -->
    <!-- 此处注意，id和dao接口中的方法名相同，resultType是返回数据的全限定类名，parameterType是所需参数的全限定类名。 -->
    <!-- 传递来的参数以#{[参数名]}的方式写进sql语句中 -->
    <select id="findAll" resultType="com.bobo.second.domain.User">
        select * from user ;
    </select>
    <select id="findById" parameterType="INT" resultType="com.bobo.second.domain.User">
        select * from user where id = #{id};
    </select>
    <select id="findMaxId" resultType="INT">
        select max(`id`) from `user`;
    </select>
    <insert id="insert" parameterType="com.bobo.second.domain.User">
        insert into user values(#{id},now(),now(),#{name},#{gender});
    </insert>
    <update id="update" parameterType="com.bobo.second.domain.User">
        update user set `gmt_modified`=now(),`name`=#{name},`gender`=#{gender} where id = #{id};
    </update>
    <delete id="deleteById" parameterType="java.lang.Integer">
        delete from user where id=#{id};
    </delete>
</mapper>
~~~
>此处应保持映射文件地址的包名和所映射的dao接口相同，命名相同

到此使用mybatis连接数据库的准备工作就完成了，接下来只需要调用dao接口的方法就可以实现对数据库中的数据的增删改查。
<div class="title3">2.3 使用mybatis</div>

这里我们需要用到之前创建的配置文件，创建dao接口的代理对象。

首先通过构造者模式，使用“建筑图纸”（主配置文件）创建一个sqlSessionFactory工厂。再通过工厂模式生产SqlSession对象（隐藏细节+解耦合），最后通过代理模式，用sqlSession创建Dao接口的代理对象。我们通过调用这个代理对象，可以实现我们所期望的方法。
~~~java
        //1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatisConfig.xml");
        //2.创建SqlSessionFactory 工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        //3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();
        //4.使用sqlSession创建Dao接口的代理对象
        UserDaoService userDaoService = session.getMapper(UserDaoService.class);
        //5.使用代理对象执行方法
        List<User> users = userDaoService.findAll();
        for(User user:users){
            System.out.println(user);
        }
        //6.释放资源
        session.close();
        in.close();
~~~
此时，如果新建一个只返回数据的控制类，并将（使用代理对象之前的步骤放进构造方法里）就可以通过页面的（通过链接传递参数）实现增删改查了。
>如果方法的内容更改了数据库的内容，则需要在执行方法后执行“事务的提交”，即`        session.commit();`
<div class="title3">2.4 创建业务层</div>

虽然可以在控制类里直接操作dao接口，但我们不这样做。因为控制类负责接收页面的请求，并返回所需要的数据（表现层），（通过调用一些方法）（是面对外部恶势力进攻的第一层屏障），所以如果把一些无关的步骤放在控制类里有一种膈应的感觉。而持久层（dao接口）仅仅负责将sql语句变成java程序内部可调用的方法，也不负责提供无关的步骤。因此我们需要建立一个中间的层级，控制类需要方法时调用业务层的方法，即可实现最后的目的。同时，如果要修改一个方法，我们无需改动控制类和dao接口，只需要改动业务层的代码（解耦合，隐藏细节）

~~~java
// 接口
package com.bobo.second.service;

import com.bobo.second.domain.User;
import java.util.List;

public interface UserCRUDService {
    void add(String name,String gender);
    void delete(int id);
    void update(User user);
    User find(int id);
    List<User> findAll();
}
~~~
~~~java
// 实现类
package com.bobo.second.service.impl;

import com.bobo.second.dao.UserDaoService;
import com.bobo.second.domain.User;
import com.bobo.second.service.UserCRUDService;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserCRUDServiceImpl implements UserCRUDService {
    public SqlSessionFactory sqlSessionFactory;
    public SqlSession session;
    public UserDaoService userDaoService;
    public UserCRUDServiceImpl() throws Exception{
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        sqlSessionFactory = builder.build(Resources.getResourceAsStream("mybatisConfig.xml"));
        session = sqlSessionFactory.openSession();
        userDaoService = session.getMapper(UserDaoService.class);
    }

    @Override
    public void add(String name,String gender) {
        User user = new User();
        user.setId(userDaoService.findMaxId()+1);
        user.setName(name);
        user.setGender(gender);
        userDaoService.insert(user);
        session.commit();
    }

    @Override
    public void delete(int id) {
        userDaoService.deleteById(id);
        session.commit();
    }

    @Override
    public void update(User user) {
        userDaoService.update(user);
        session.commit();
    }

    @Override
    public User find(int id) {
        return userDaoService.findById(id);
    }

    @Override
    public List<User> findAll() {
        return userDaoService.findAll();
    }
}

~~~
>此处类上方添加注解`@Service`目的和控制类添加注解`@Controller`一样,是为了让Spring boot 自动生成该类的一个对象，（在使用时无需自己再创建一个对象）（不会多次创建对象）

此时，如果新建一个只返回数据的控制类，并用依赖注入注入`UserCRUDService`就可以通过页面的（通过链接传递参数）和调用该对象的方法实现增删改查了。
~~~java
    @Autowired
    public IndexController(UserCRUDService userCRUDService) {
        this.userCRUDService = userCRUDService;
    }
~~~
>依赖注入

---
<div class="title1">三、使用freemaker实现动态页面生成</div>

---
<div class="title3">3.1 freeMaker在线手册</div>

[手册地址](http://freemarker.foofun.cn/)
<div class="title3">3.2 配置</div>

首先同样，需要在pom.xml里导入freemaker的依赖。
~~~xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-freemarker</artifactId>
		</dependency>
~~~
(看包名应该是Spring集成的版本)  
然后在Spring的配置文件application.properties里进行freemaker的配置。或者可以在同个目录下新建一个application.yml进行配置。
>application.properties和application.yml的作用是相同的，但是语法不同。个人觉得yml更简洁一点。
~~~yml
# application.yml
spring:
  freemarker:
    suffix: .ftl                    # 设置模板后缀名
    content-type: text/html                      # 设置文档类型
    charset: UTF-8                               # 设置页面编码格式
    cache: false                                 # 设置页面缓存
    template-loader-path: classpath:/templates   # 设置ftl文件路径
~~~
>模板后缀名可以用html。但这样freemaker的指令无法实行。

到此freemaker的配置就结束了。我们可以在templates文件夹内新建模板，然后在JAVA程序内使用ModelAndView类来返回一个动态页面。
<div class="title3">3.3 使用freeMaker</div>

+ 关于值的传递。  
在JAVA源程序中，我们可以将需要使用的数据和他的名字添加到`modelAndView`对象中，再在模板内使用`${name}`的方式调用该数据.在具体操作中，我选择在控制类的属性里加一个Map,然后把需要用的数据加到这个Map里，再把Map加到modelAndView中。以此来保存数据。
~~~java
        modelAndView.setViewName("index");
        modelAndView.addObject("s",map);
~~~
最后返回`modelAndView`，即可通过`ViewName`(模板名)和modelAndView中的数据构成静态页面。
+ 关于指令  
指令的形式和html标签很相似，但多了个#号。
~~~html
<#if a == b>
<!-- 当条件满足时，内部的语句会被输出到静态页面中 -->
<!-- TODO -->
</#if>
<#list animals as animal>
<!-- animals为列表，animal是为该列表里的每个元素取的别称。然后在这个指令内部，会把每个元素用同样的语句输出一遍 -->
</#list>
~~~
>也可以用`[#if]`的形式代替`<#if>`，但是同一个模板内只能采用一种形式。

然后我们可以用这些东西来写FreeMaker的程序了。
~~~html
<!-- index.ftl -->
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="css/index.css">
    <title>报名系统</title>
</head>

<body>
    <div class="left">
        <ul>
            <li><form action="/login/button"><input type="text" name="o" value="insert"><button type="submit">增加</button></form></li>
            <li><form action="/login/button"><input type="text" name="o" value="delete"><button type="submit">删除</button></form></li>
            <li><form action="/login/button"><input type="text" name="o" value="update"><button type="submit">更改</button></form></li>
            <li><form action="/login/button"><input type="text" name="o" value="find"><button type="submit">查询</button></form></li>
            <li><form action="/login/button"><input type="text" name="o" value="findAll"><button type="submit">显示</button></form></li>
        </ul>
    </div>
    <div class="right">
        <form class="insert" <#if s.o != "insert">style="display:none"</#if> action="/login/insert">
            <input type="text" placeholder="name" name="name">
            <input id="female" type="radio" value="female" name="gender">
            <label for="female">女</label>
            <input id="male" type="radio" value="male" name="gender">
            <label for="male">男</label>
            <button class="shadow" type="submit">提交</button>
        </form>
        <form class="delete" <#if s.o != "delete">style="display:none"</#if>  action="/login/delete">
            <input type="text" placeholder="id" name="id">
            <button class="shadow" type="submit">提交</button>
        </form>
        <form class="update" <#if s.o != "update">style="display:none"</#if>  action="/login/update">
            <input type="text" placeholder="name" name="name">
            <input id="female" type="radio" value="female" name="gender">
            <label for="female">女</label>
            <input id="male" type="radio" value="male" name="gender">
            <label for="male">男</label>
            <button class="shadow" type="submit">提交</button>
        </form>
        <form class="find" <#if s.o != "find">style="display:none"</#if>  action="/login/find">
            <input type="text" placeholder="id" name="id">
            <button class="shadow" type="submit">提交</button>
        </form>
        <form class="findAll" <#if s.o != "findAll">style="display:none"</#if>  action="/login/findAll">
            <button class="shadow" type="submit">开始查询</button>
        </form>
        <#if s.isLook == true>
        <ul>
            <li class="insert"  <#if s.o != "insert">style="display:none"</#if> >增加成功！</li>
            <li class="delete" <#if s.o != "delete">style="display:none"</#if> >删除成功！</li>
            <li class="update" <#if s.o != "update">style="display:none"</#if> >更改成功！</li>
            <li class="find" <#if s.o != "find">style="display:none"</#if> >
                <div class="id">id:${s.user.id}</div>
                <div class="name">姓名：${s.user.name}</div>
                <div class="gender">性别：${s.user.gender}</div>
            </li>
            <li class="findAll" <#if s.o != "findAll">style="display:none"</#if> >
                <ul>
                    <#list s.list as user>
                        <li>
                            <div class="id">id:${user.id}</div>
                            <div class="name">姓名：${user.name}</div>
                            <div class="gender">性别：${user.gender}</div>
                        </li>
                    </#list>
                </ul>
            </li>
        </ul>
        </#if>
    </div>

</body>

<script type="text/javascript" src="js/jquery-3.4.1.min.js"></script>
</html>
~~~
>当然这个JQ最后没用上。。感觉不熟练啊！
~~~java
package com.bobo.second.control;

import com.bobo.second.domain.User;
import com.bobo.second.service.UserCRUDService;
import org.apache.ibatis.annotations.Param;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Controller
@RequestMapping("login")
public class IndexController {
    public UserCRUDService userCRUDService;
    public Map<String,Object> map = new HashMap<>();
    ModelAndView modelAndView;
    @Autowired
    public IndexController(UserCRUDService userCRUDService) {
        this.userCRUDService = userCRUDService;
        User user = new User();
        user.setId(0);
        user.setName("null");
        user.setGender("male");
        map.put("user",user);
        List<User> list = new ArrayList<>();
        list.add(user);
        map.put("list",list);
        map.put("o","insert");
        map.put("isLook",false);
    }

    @RequestMapping("")
    public ModelAndView index(ModelAndView modelAndView){
        modelAndView.setViewName("index");
        modelAndView.addObject("s",map);
        return modelAndView;
    }

    @RequestMapping("insert")
    public String insert(@Param("name")String name,@Param("gender")String gender){
        userCRUDService.add(name,gender);
        map.put("isLook",true);
        return "redirect:../login";

    }
    @RequestMapping("find")
    public String find(@Param("id")int id){
        User user = userCRUDService.find(id);
        map.put("user",user);
        map.put("isLook",true);
        return "redirect:../login";
    }
    @RequestMapping("findAll")
    public String findAll(){
        List<User> list = userCRUDService.findAll();
        map.put("list",list);
        map.put("isLook",true);
        return "redirect:../login";
    }

    @RequestMapping("button")
    public String button(@RequestParam("o")String s){
        map.put("o",s);
        map.put("isLook",false);
        return "redirect:../login";
    }

}
~~~
>+ 这里依赖注入`@AutoWired`添加在构造函数上，是因为有时候直接添加在属性上会出现注入失败的原因。
>+ 这里传递isLook属性是为了模板确定是不是要显示结果。传递o属性是为了告诉模板现在是在进行什么操作，用来遮掩不需要显示的页面。
>+ 使用了重定向redirect用来重新加载最初的页面，进行数据的传递。这里之所以采用`"../login"`而不是`""`是因为后者实际上访问的是`localhost:8080/login/`而不是`localhost:8080/login`,而这会导致CSS的路径加载了`login/css/index.css`而不是`css/index.css`,会出现加载错误的情况。

---
<div class="title1">四、附录</div>

---
<div class="title3">4.1 css文件</div>

~~~css
/* css/index.css */
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    list-style: none;
}

.shadow,form{
    box-shadow: 5px 3px 2px 2px rgba(39, 61, 57, 0.76);
}
.left{
    display: flex;
    width:50px;
}
body{
    display: flex;
}
.left form input{
    display: none;
}
~~~
>所有静态资源（css,js（如果有的话））都在static包下。
<div class="title3">4.2 打包并运行</div>

+ 打包  
Ideaj 右上角有一个MAVEN字样，点开来，点击M按钮（+号右边第二个）,选择执行`mvn clean package`即可进行打包。生成的jar包在target文件夹内。
+ 运行  
进入到打出来的包所在的文件夹内。使用`java -jar [包名]`命令运行。
>若运行失败，检查环境配置是否和程序所使用的的环境一致。（比如一个Java 8 一个Java 11）