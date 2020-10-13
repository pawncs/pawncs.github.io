---
title: CSS补充.md
date: 2020-09-14 15:32:11
categories:
- html,css,js
tags:
- CSS
- study
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

CSS引用形式
=========

行内式，☆内部样式(style标签)，外部样式


选择器
=====
>后代选择器（p a）  
交集选择器(a.special)  
子代选择器(a>.special)  
并集选择器(a,.special)
兄弟选择器(a+.special)

字体
===
加粗：
~~~
font-weight:bold;
~~~
>bold释义：大胆的，英勇的；黑体的；厚颜无耻的；险峻的  
>本专业释义：粗体，粗的

优先级
====  
>当选择器重复选取时的优先级

id选取>class选取>标签选取
同级的时候比较过程中最高级的数量（例如用了几次id选取）


padding,border和margin
========================
## padding

可在导航栏发挥大用（不让贴着边）  
padding:20px 30px;  
指上下20，左右30。  
默认参数顺序为：上右下左（顺时针）  

>box-sizing:bording-box或content-box;

border:直线，虚线
>border-style:solid,dashed    

border例
~~~css
border-top:1px solid black;  
border-top:none; 
//不显示
~~~

·阴影

box-shadow:2px 2px 2px 1px rgba(0,0,0,0.2);  
x偏移 y偏移 阴影模糊半径 阴影扩散半径 阴影颜色


.子级元素撑开父级元素宽度
=======================
min-width(作用存疑)或者添加浮动

超出部分隐藏：overflow:hidden

background
===========
~~~
    background-size:cover,content
    cover为拉伸，content为不改变长宽比的情况下最大
    也可以直接写百分数

    设置渐变色：
    background:linear-gradient(方向，开始色号，结束色号);
~~~

display:none,,block,inline,inline-block  
inline-block时，将父元素的font-size设为0，消除空白折叠现象


position
========
>relative（相对定位）    

相对自己进行偏移
>absolute（绝对定位）

相对最先遇到的非static的祖先元素进行偏移
>fixed（固定定位）  

不随着页面滚动而改变

>sticky(粘性定位)  

看起来很好玩，，——经过的时候回粘下来




有关遮挡：z-index:越大离观察者越近

去除浏览器自带样式
===============
~~~css
html,
body {
    height: 100%;
    margin: 0;
}

ul,
li {
    margin: 0;
    padding: 0;
}
li {
    list-style: none;
}

h1,
h2 {
    margin: 0;
}

p {
    margin: 0;
    padding: 0;
}

/* 浏览器都有自己的默认样式，以上部分为清除浏览器默认样式 */
~~~


CSS中的计算：
===========
calc:
~~~css
width:calc(100% - 480px);
~~~


伪元素
=====
标签内部的前面或后面添加行内元素（span）

伪类
=====
事件伪类：

hover : 光标覆盖时  
active : 光标点击时

注意：hover一定要在active之前

focus:获得焦点后


CSS装饰
================

cursor :   
pointer,,添加小手手；default;text;等一切鼠标悬浮的效果




浮动布局flex
===========
justify-content:定义了项目在水平方向上的对齐方式  
>flex-start,flex-end,center,space-between,space-around,space-evenly

align-items属性定义项目在垂直方向上如何对齐
>flex-start,flex-end,center,baseline(以项目第一行文字为基线对齐),stretch(默认)(拉伸)

flex-direction:项目的排列方向
>column(纵向),row,row-reverse,column-reverse

flex-wrap:压缩时换不换行
>nowrap:不换行；wrap换行;wrap-reverse换行后第一行在下面

容器里放不下时是否压缩：
flex:(与其他设置在flex容器上的属性不同，这个属性设置在项目上)
>none,1(自动充满剩余空间)

多行文本省略
==========
~~~css
/* 隐藏超出部分 */
overflow : hidden;
/* 文本超出就用省略号 */
text-overflow: ellipsis;
/* 把对象作为弹性伸缩盒子模型显示 */
display: -webkit-box;
/* WebKit内核的浏览器的私有属性，设置文本超出2行就用省略号 */
-webkit-line-clamp: 2;
/* WebKit内核的浏览器的私有属性，设置或检索伸缩盒对象的子元素的排列方式 */
-webkit-box-orient: vertical;
~~~

CSS响应式
========
使一个网站能兼容多个终端，而不是为每个终端做一个版本
>响应式布局主要通过规定<strong>特定的宽度范围使用特定的布局</strong>来实现在不同的设备上应用不同的布局

### 媒体查询 `@media`

当满足特定条件时，才使用对应的CSS属性块
~~~css
@media screen and (max-width: 600px) {
  .topnav {
    flex-direction: column;
  }
  .column {
    width: 100%;
  }
}
~~~

# CSS关键帧@keyframes（动画）

[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes)