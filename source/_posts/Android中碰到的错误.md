---
title: Android中碰到的错误
date: 2020-08-21 02:43:34
categories:
- Android
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
<div class="title2">一、Realm篇</div>

-----
<div class="title4">@PrimaryKey</div>

只有Integer和String和其相关类能作为主键。
`Date`不行...(查了好几天)

……  
……  
章结尾

-----
<div class="title2">附录、显示错误细节</div>

-----

在terminal端口输入
~~~
gradlew compileDebug --stacktrace -info 
~~~
查错误详情  

或
~~~
gradlew compileDebug --stacktrace -debug
~~~
再
~~~
gradlew compileDebugSources --stacktrace -info
~~~