---
title: CSS简单图形绘制
date: 2021-01-29 14:03:39
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

.example1-1 {
  width: 10px;
  height: 1.5px;
  background: #2F9842;
  transform: rotate(45deg);
  border-radius: 1.5px;
  margin-top: 6.0113154173px;
  margin-bottom:9.5px;
  position: relative;
  left: 7px;
}
.example1-1::before {
  position: relative;
  display: block;
  content: "";
  width: 10px;
  height: 1.5px;
  background: #2F9842;
  transform: rotate(90deg);
  border-radius: 1.5px;
  left: 4.25px;
  top: 4.25px;
}
.example1-1-2{
    border: 10px solid;
  border-top-color: #2F9842;
  border-left-color: #4A4A4A;
  border-right-color: yellow;
  border-bottom-color: red;
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
<div class="title2">一、简单符号</div>

-----
<div class="title3">1.1 箭头</div>

<div class="example1-1"></div>
<div class="example1-1"></div>
<div class="example1-1"></div>
<div >。，。</div>
<div class="example1-1"></div>


~~~scss
.img {
      $lineWidth: 1.5px;
      $lineLong: 10px;

      &::before {
        position: relative;
        display: block;
        content: "";
        width: $lineLong;
        height: $lineWidth;
        background: #2F9842;
        transform: rotate(90deg);
        border-radius: $lineWidth;
        left: -($lineWidth - $lineLong)/2;
        top: -($lineWidth - $lineLong)/2;

      }

      width: $lineLong;
      height: $lineWidth;
      background: #2F9842;
      transform: rotate(45deg);
      border-radius: $lineWidth;
      margin-top: ($lineLong - $lineWidth)/1.414;
      margin-bottom: ($lineLong - $lineWidth)/1.414;
      position: relative;
      left: 7px;
      }
~~~

+ 思路是先画出两个一模一样的矩形，再进行旋转拼凑出箭头。
+ 网上查过很多教程，大多采用边框画三角形的方式（border宽度等于块的宽度时几条边挤压在一起画出三角形），再用遮叠来画。如下图所示，然后更改各条边的颜色或透明度。
  
<div class="example1-1-2"></div>

>然而这种方式并不能画出圆滑的线，在底图变化的情况下也不好用，所以采用上面的方式。

<!-- <div class="title4">小标题</div>

……  
……  
章结尾 -->

-----