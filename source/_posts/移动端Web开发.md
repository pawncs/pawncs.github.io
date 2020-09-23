---
title: 移动端Web开发
date: 2020-09-15 13:00:04
categories:
- html,css,js
tags:
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

-----
<div class="title2">一、认识meta </div>

-----

>`<meta>` 在 `<head>` 标签内
<div class="title3">1.1 meta元素</div>

>元数据信息：meta-infortion  
>元数据：用来描述数据的数据  

`<meta>` 是一个自闭合元素，只有开始标签，没有结束标签。

对meta元素进行设置指的是通过标签属性进行设置。
1. charset的属性
    + utf-8   
   (UTF-8是HTML5的默认属性，涵盖了世界上几乎所有字符和符号)(没有他，中文就是乱码)
2. viewport的属性
    + width：viewport的宽度
    + height：viewport的高度
    + initial-scale：初始的缩放比例
    + minimum-scale：允许用户缩放到的最小比例
    + maximum-scale：允许用户缩放到的最大比例
    + user-scalable:用户是否可以手动缩放

~~~html
<!-- 声明文档使用的字符编码 -->
<meta charset="utf-8">
~~~

~~~html
<!-- 设置移动端视图 -->
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
<!-- H5页面窗口自动调整到设备宽度，并禁止用户缩放页面 -->
~~~

-----
-----
<div class="title2">二、认识新单位 rem/em</div>

-----
>rem 是一个相对长度单位。在不同的环境中，1rem可以转化为不同的px的值。

<div class="title3">2.1 单位rem和px之间的转换</div>

rem和px之间的转换比取决于页根元素(`<html></html>`)的字体大小.1 rem = 根元素的字体大小

例：浏览器默认字体大小为16px,则2.5rem = 2.5 * 16 px = 40px

> 一般浏览器默认为16px.最小显示为10px.即设置10px以下的数值一般是没有用的，显示的时候都会按10px显示，除非用到了CSS的transform。

<div class="title3">2.1 单位em</div>

rem是根据根元素的字体大小转换的，em是根据本级元素的字体大小转换的（当本级元素未确定时，根据父元素）
~~~scss
.parent {
  // 父元素的字体大小为 20px
  font-size: 20px;

  > .child {
    // 子元素的字体大小为 0.7 * 20px = 14px
    font-size: 0.7em; 
    // 特别注意：子元素的宽度为 10 * 14px = 140px
    width: 10em;
  }
}
~~~
-----
-----
<div class="title2">三、认识新单位 vw/vh</div>

-----

+ 1vw = 1/100 视口宽度 
+ 1vh = 1/100 视口高度

<div class="title3">3.1 模态框</div>

指页面中弹出的窗口。此时需要给整个视口加上半透明覆盖层。
~~~css
// 将蒙层的宽高设置为视口的宽度和高度
width: 100vw;
height: 100vh;
~~~