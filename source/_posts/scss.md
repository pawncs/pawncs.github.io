---
title: scss
date: 2020-09-12 16:49:12
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
<div class="title2">一、SASS</div>

-----
<div class="title3">1.1 什么是SASS</div>

+ SASS是一款CSS的预编译器，定义了一种新的编程语言，需要先编译成CSS才能被浏览器理解。
+ SASS比CSS更像一种编程语言，有变量，函数，控制语句，导入，嵌套等高级功能。
+ 类似的预编译器还有`less`,`Stylus`等
<div class="title4">有了这些功能，SASS可以</div>

1. 提供变量，实现一键替换主题色之类的功能
2. 用嵌套写法减少选择器的重复书写
3. 用混用功能解决代码复用问题
4. 用函数进行复杂的运算
5. 把央视代码模块化，减少不同的模块的代码间不必要的相互影响，提高代码安全性
6. ……

<div class="title3">1.2 安装</div>
<div class="title4">安装sass</div>

参考 [sass文档](https://sass.bootcss.com/documentation) 安装到本地，然后配置到ideaj,vs等。（自行百度）

在HTML文件中直接引入编译后的CSS文件即可

<div class="title4">Sass和Scss</div>

Scss是Sass 3 为了兼容CSS引入的新语法（任何标准的CSS3样式表都是具有相同语义的有效的Scss文件）
>文件后缀为`.scss`

-----
<div class="title2">二、变量</div>

-----
<div class="title3">2.1 变量</div>

提供了SassScript的新功能。可用于任何属性，允许属性使用变量、算术运算等额外功能  
变量以“$”开头，赋值方法与CSS一样:  
` $width: 10px;`   

<div class="title4">使用变量</div>

~~~scss
#main {
  width: $width;
}
~~~

变量类型为字符串一般不需要双引号，除非某些特殊情况，如有双斜杠“//”，则需要单引号或者双引号（因为`//`表示注释）

<div class="title4">定义类似于数组的变量</div>

` $animals: puppy kitty chick;`

可配合循环语句使用

<div class="title4">支持简单计算</div>

~~~scss
$width: 10px;

#main {
  width: $width / 2;
}
~~~
因此可以方便的定义长宽比固定的元素，如
~~~scss
$width: 10px;

#main {
  width: $width / 2;
  height: $width * 2;
}
~~~
<div class="title3">2.2 插值法</div>

`#{}` 插值可以在Sass样式表的任何一个地方使用
~~~scss
$name: "mail";
$top-or-bottom: "top";
$left-or-right: "left";

.icon-#{$name} {
  background-image: url("/icons/#{$name}.svg");
  position: absolute;
  #{$top-or-bottom}: 0;
  #{$left-or-right}: 0;
}
~~~
>可以用插值法插入任何类型的变量，不仅仅是字符串

-----
<div class="title2">三、嵌套</div>

-----

>将共同的选择器提取出来，防止选择器过长

~~~scss
main .double .item .links {
  text-align: center;
  a {
    margin-right: 20px;
  }
}
~~~
<div class="title3">3.1 嵌套</div>
<div class="title4">嵌套规则</div>

Sass允许将一套CSS样式嵌套进另一套样式中，内层的样式将他的外层的选择器作为父选择器。

<div class="title4">父选择器 &</div>

在嵌套时，有时候需要直接使用嵌套外层的福选择器，（例如，给元素设定`hover`样式时，可以用'&'代表规则外层的父选择器）
>个人认为更像插值（可以字符串拼接）

-----
<div class="title2">四、复用：mixin/include</div>

-----

<div class="title3">4.1 未完待续</div>
<div class="title4">未完待续</div>



-----