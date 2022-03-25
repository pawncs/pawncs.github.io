---
title: git手册概览
date: 2022-03-02 09:58:38
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
<div class="title2">零、目录</div>

-----
<a href="#git-Manual-overview-1">一、起步</a>  
+ 有关概念 
+ 特点  
+ 三种状态  

<a href="#git-Manual-overview-2">二、Git基础</a>    



-----
-----
<div class="title2" id="git-Manual-overview-1">一、起步</div>

-----
<div class="title4">git有关概念</div>

+ 版本控制  
  版本控制是一种记录一个或若干文件内容的变化，以便将来查阅特定版本修订情况的系统
  + 本地版本控制系统
  + 集中化的版本控制系统
  + 分布式版本控制系统

<div class="title4">git特点</div>

git和其他版本控制系统非常相似，但在对<strong>信息的存储</strong>和<strong>认知方式</strong>上存在很大差异。  

+ git直接记录快照(cvs，svn等为差异比较)  
    + 即其他版本控制系统存储文件的变更，而git直接存储快照  
    ~~~
    git对待数据更像对待一个快照流
    ~~~
+ 近乎所有操作本地执行  
    + 在本地有完整的项目历史，可以查看之前的版本
+ Git 保证完整性
    + 存储前计算校验和
    + 事实上，Git数据库中保存的信息都是以文件内容的hash值来索引而不是文件名
+ Git 一般只添加数据
    + 一旦提交快照到Git中，就再难丢失数据

<div class="title4">git三种状态</div>

+ 已修改(`modified`)
    + 已修改了文件，但还未保存到数据库
+ 已暂存(`staged`)
    + 对一个已修改的文件做了标记，将提交到下次的快照
+ 已提交(`committed`)
    + 数据已经安全的保存在本地数据库中

> 与之对应git项目有了三个阶段：工作区、暂存区、Git目录
>~~~
>  Working          Staging       .git directory
> Directory          Area          (Repository)
>     |<------checkout the project------|
>     |               |                 |
>     |--Stage Fixes->|                 |
>     |               |                 |
>     |               |------Commit---->|
>~~~
>n

-----
-----
<div class="title2" id="git-Manual-overview-2">二、Git基础</div>

-----



-----