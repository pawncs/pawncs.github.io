---
title: javascript入门
date: 2020-09-25 00:33:11
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
<div class="title2">一、认识Javascript</div>

-----
JavaScript主要由以下三部分组成：
1. 核心（ECMAscript）
2. 文档对象模型(DOM)
3. 浏览器对象模型(BOM)

<div class="title3">1.1 核心（ECMAscript）</div>

>`ECMAScript`规定了这门语言的基本组成部分

主要由以下部分组成：

+ 语法
  
+ 类型
+ 语句
+ 关键字
+ 保留字
+ 操作符
+ 对象

有了这些，`JavaScript`可以完成基本的逻辑以及数据处理。
<div class="title3">1.2 文档对象模型（DOM）</div>

>`DOM`的功能为获取`html`标签，并对标签进行其他操作（增删样式，添加事件）。

这些功能的实现基于以下接口：
+ DOM 遍历和范围：  
  可以找到页面中所有标签

+ DOM 事件：  
  可以给标签添加事件（如给图片添加拖动事件）
+ DOM 样式：  
  可以更改页面中所有元素的样式，如更改某一段文字的颜色
<div class="title3">1.3 浏览器对象模型(BOM)</div>

>`BOM`处理和浏览器相关的东西

如：
+ 弹出新窗口功能

+ 移动、缩放、关闭浏览器窗口的功能
+ 给用户提供显示器分辨率的功能
+ 提供浏览器信息

<div class="title3">1.4 JavaScript的书写位置</div>

>和`css`一样，分为`HTML`内部和外部。

<div class="title4">内部</div>

使用`script`标签内嵌。
~~~html
// script标签嵌入JavaScript代码
<script>
    // JavaScript代码
    let name = "Bob";
    function(){
        console.log("我的名字叫："+name);
    }
</script>
~~~

>这里的规范为：`<script>`标签的位置位于`body`标签内部，并保证是末尾。

其实该标签写在哪都可，但是在进行`DOM`操作时，如果不注意位置，会出现奇怪的报错。
<div class="title4">外部</div>

代码写在`XXX.js`文件中，由引入标签引入即可。
~~~html
<script src='index.js'></script>
~~~
书写位置与内部书写方式一致，即在`body`标签的内部，但是在末尾

-----
-----
<div class="title2">二、基础数据类型</div>

-----
<div class="title3">2.1 变量</div>

在`JavaScript`中定义变量的关键词有两个：`let` , `const`。
~~~js
let name = 'Will Smith';
console.log(name);
~~~

>曾经也有关键词 `var`， 但其会导致未知的错误，因此已经不常用了

<div class="title4">let 和 const 的异同</div>

+ const定义的变量只能赋值一次，let定义的变量可以多次赋值
+ const定义的变量需要赋值，否则会报错；let定义的变量可以不赋初始值（默认为`undefined`）。
<div class="title3">2.2 数值类型</div>

>整数、浮点数、NaN(Not a Number),等。

<div class="title4">整数</div>

八进制和十六进制，十进制
~~~js
let number1 = 010; // 八进制的8
let number2 = 0x11; // 十六进制的17
let number3 = 10;//十进制的10
~~~
<div class="title4">浮点数</div>

小数形式，科学计数法形式
~~~js
let floatNumber1 = 1.2;

let floatNumber2 = 3.33e7;//3.33*10^7
~~~
浮点数最高精度为17位。但要注意其精度丢失的问题。（0.1 + 0.2 != 0.3）
<div class="title4">NaN</div>

当两个变量执行了运算，且返回的结果仍然应该是数字类型，但是执行的数学运算未成功，就会返回`NaN`.
~~~js
let a = 'number';
let b = 10;
let c = a / b;
console.log(c); // NaN
console.log(typeof c); // number
~~~

其他会出现`NaN`的情况:
+ 0/0
+ 字符串乘以数字
+ `NaN`和其他数进行运算
<div class="title3">2.3 类型转换/字符串拼接</div>
<div class="title4">隐式转换</div>

数字字符串(`string`)和数字(`number`)做加法运算，数字会隐式转换为字符串  
数字字符串和数字做非加法运算，字符串会隐式转换为数字
~~~js
console.log(20+'20'); // 2020
// 调换位置亦可
console.log('20'+20); // 2020

console.log('20'-10); // 10
console.log(10*'10'); // 100
console.log(10/'2'); // 5
~~~
<div class="title4">强制类型转换</div>

`parseInt()`,`parseFloat()`

<div class="title4">字符串拼接</div>

用+号。不过我个人更喜欢模板字符串。即使用反引号` `` `将${}括起来这种。
<div class="title3">2.4 运算符</div>
<div class="title4">相等/全等</div>

区别：相等运算时可能会进行隐式转换
~~~js
let number1 = '45';
let number2 = 45;
console.log(number1 == number2); // true
~~~
而全等运算会判断类型
~~~js
let number1 = '45';
let number2 = 45;
console.log(number1 === number2); // false
~~~
推荐使用全等。

<div class="title3">2.5、布尔类型/条件判断</div>

特别：  
| 数据类型 | true | false |
| :----: | :----: | :----: |
| 字符串 | 非空字符串 | ""(空字符串) |
| 数字 | 非零数字 | 0 |

-----