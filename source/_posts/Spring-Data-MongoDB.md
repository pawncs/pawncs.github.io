---
title: Spring_Data_MongoDB
date: 2020-10-14 16:26:38
categories:
- Java Web
tags:
- dictionary
- MongoDB
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
~~~
╔═══════╦ 一、配置  
║       ╠═══ 1.1 添加依赖  
║       ╚═══ 1.2 配置数据库  
║   
╠═══════╦ 二、CRUD（增删改查）  
║       ╠═══ 2.1 Create —— 增加数据  
║       ╠═══ 2.2 Retrieve —— 查询数据  
║       ╠═══ 2.3 Update —— 更新数据  
║       ╚═══ 2.4 Delete —— 删除数据  
║   
╠════════ 三、QUERY  
║       
╚════════ 四、分页  
~~~

-----
<div class="title2">一、配置</div>

-----
<div class="title3">1.1 添加依赖</div>

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
~~~

<div class="title3">1.2 配置数据库</div>

修改 `application.properties` 文件，或者新建同名yml文件，使用Yml语法。
~~~properties
# 购买的云服务器的公网 IP
spring.data.mongodb.host=xxx.xxx.xxx.xxx
# MongoDB 服务的端口号
spring.data.mongodb.port=27017
# 创建的数据库及用户名和密码
spring.data.mongodb.database= 数据库名
spring.data.mongodb.username= 用户名
spring.data.mongodb.password= 密码
~~~

-----
<div class="title2">二、CRUD</div>

-----
<div class="title3">2.1 Create —— 增加数据</div>

~~~java
import org.springframework.data.mongodb.core.MongoTemplate;

  @Autowired
  private MongoTemplate mongoTemplate;

  public void test() {
    Class1 class1 = new Class1();
    class1.setAttribute("添加某属性");
    // 添加其他属性
    mongoTemplate.insert(class1);
  }
~~~
系统会自动为主键 `id` 属性赋值。

<div class="title3">2.2 Retrieve —— 查询数据</div>

此处ID为主键ID。
~~~java
//根据id，返回对象
mongoTemplate.findById(id, ClassName.class);
//根据Query,返回列表
mongoTemplate.find(query, ClassName.class);
//有关Query如何创建放在第三章
~~~

<div class="title3">2.3 Update —— 更新数据</div>

~~~java
// 修改 id=1 的数据
Query query = new Query(Criteria.where("id").is("1"));

// 把歌名修改为 “new name”
Update updateData = new Update();
updateData.set("name", "new name");

// 执行修改，修改返回结果的是一个对象
UpdateResult result = mongoTemplate.updateFirst(query, updateData, ClassName.class);
// 修改的记录数大于 0 ，表示修改成功
System.out.println("修改的数据记录数量：" + result.getModifiedCount());
~~~

先使用条件对象`Criteria`构建条件对象`Query`的实例，再用修改对象`Update`设置需要修改的字段。最后用`mongoTemplate`的方法完成修改

<div class="title3">2.4 Delete —— 删除数据</div>

函数`mongoTemplate.remove()`删除数据。
~~~java
Song song = new Song();
song.setId(songId);//主键ID

// 执行删除
DeleteResult result = mongoTemplate.remove(song);
// 删除的记录数大于 0 ，表示删除成功
System.out.println("删除的数据记录数量：" + result.getDeletedCount());
~~~

-----
<div class="title2">三、Query</div>

-----

Query条件对象的实例根据Criteria条件对象的实例构建
`Query query = new Query(criteria);`

<div class="title4">Criteria单一使用</div>

`Criteria criteria1 = Criteria.where("条件字段名").is("条件值")`

<div class="title4">Criteria组合使用</div>

~~~java
//or
Criteria criteria = new Criteria();
criteria.orOperator(criteria1, criteria2);
//and
Criteria criteria = new Criteria();
criteria.andOperator(criteria1, criteria2);
~~~

-----
<div class="title2">四、分页</div>

-----

让查询支持分页：只需调用`PageRequest.of()`构建一个分页对象，而后注入查询对象即可。
~~~java
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

//第一个参数是页码（从0开始计数），第二个参数是每页的数量。
Pageable pageable = PageRequest.of(0 , 20);
query.with(pageable);
~~~

计算查询总数，以计算总共多少页。
实现分页功能还需：
+ 调用`count(query,XXX.class)`方法获取查询总数。
+ 根据结果，分页条件，总数三个数据，构建分页器对象

~~~java
import org.springframework.data.domain.Page;
import org.springframework.data.repository.support.PageableExecutionUtils;

// 总数
long count = mongoTemplate.count(query, Song.class);
// 构建分页器
Page<Song> pageResult = PageableExecutionUtils.getPage(songs, pageable, new LongSupplier() {
  @Override
  public long getAsLong() {
    return count;
  }
});
~~~

-----