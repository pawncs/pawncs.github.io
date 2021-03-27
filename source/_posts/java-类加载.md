---
title: java-类加载
date: 2021-03-23 23:06:48
categories:
tags:
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
类加载阶段
--
~~~
 加载(loading)
  |
  V
 连接(linking)（验证(verification)-->准备(preparation)-->解析(resolution)）
  |
  V
初始化(initialization)
  |
  V 
 使用 (using)
  |
  V
 卸载 (unloading)
~~~


-----