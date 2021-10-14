---
title: kafka 初探
date: 2021-08-18 14:22:12
categories:
- Java Web
tags:
- MQ
- kafka 
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
<div class="title2">一、Kafka简介与安装</div>

-----
<div class="title3">1.1 Kafka是什么</div>

<div class="title4">消息系统</div>

`Kafka`允许<strong>发布和订阅数据</strong>。

> kafka不同之处：他作为分布式系统，以集群方式运行，可以自由伸缩，同时提供数据传递保证——可复制、持久化

<div class="title4">存储和持续处理大型数据流</div>

`Kafka`可以存储和持续处理大型数据流，并保持持续性的低延迟。在这一点上，可将其看成实时版的`Hadoop`.
> 其低延迟特点更适合用在核心业务应用上，当业务事件发生时，Kafka能及时对这些事件作出响应。

<div class="title4">实时流平台</div>

`Kafka` 是面向实时数据的流平台。不仅可以将现有的应用程序和数据联系起来，还能用于加强这些触发相同数据流的应用。
> 这些数据流是现代数字科技公司的核心，和现金流一样。

<div class="title3">1.2 Kafka 安装</div>


-----
-----
<div class="title2">二、Kafka组成介绍</div>

-----
<div class="title3">1.1 节标题</div>

+ 要点
+ 要点
<div class="title4">小标题</div>

……  
……  
章结尾

-----
-----
<div class="title2">三、Kafka流式处理</div>

-----
<div class="title3">1.1 节标题</div>

+ 要点
+ 要点
<div class="title4">小标题</div>

……  
……  
章结尾

-----