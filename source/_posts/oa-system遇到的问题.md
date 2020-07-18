---
layout: clone
title: oa_system遇到的问题
date: 2020-07-16 17:20:19
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

[oasys:办公自动化项目地址](https://gitee.com/aaluoxiang/oa_system)

<div class="title3">前言</div>

在将该项目clone到本地后，发现无法运行。在搜索引擎和朋友的帮助下，做出以下措施。

<div class="title4">增加Maven镜像地址：增加下载速度</div>

在maven的安装目录下找到settings.xml文件。
>D:\maven\apache-maven-3.6.3-bin\apache-maven-3.6.3\conf

然后在其`<mirrors>`标签内添加阿里镜像
~~~xml
    <mirror>
      <id>nexus-aliyun</id>
      <mirrorOf>*</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
~~~
>下载速度和飞一样
mvn -U idea:idea
<div class="title4">解决plugin飘红</div>

在解决了这个问题后，仍然存在plugin中存在红色。经过百度后查得，需要在控制台中输入一下`mvn -U idea:idea`来解决

<div class="title4">数据库</div>

修改application.properties为自己的数据库

再运行SQL语句

此时就可以启动项目了