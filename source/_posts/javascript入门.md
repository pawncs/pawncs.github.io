---
title: javascript入门
date: 2020-10-01 23:33:11
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
-----
<div class="title2">三、数组</div>

-----

用`[]`表示，可以储存不同类型的数据。
其他和Java，C等差不多，此处略。
~~~js
let arr = [1,'第一名',true,[2,'第二名',false]];
~~~
<div class="title3">3.1 增删改查</div>

~~~js
//增-push
变量名.push('要添加的值');
//一次多个
schools.push('河海大学', '大连理工大学', '哈尔滨工业大学');

//增加在开头 -unshift
array.unshift(`待增加元素`);
~~~

~~~js
//从后往前删 pop
schools.pop();

//从前往后删 shift
schools.shift();

//删除指定位置 splice
//我更愿意理解为分割，返回从第一个参数为下标往后数第二个参数为个数的所有内容。
school.splice(1);
school.splice(0,2);
~~~

~~~js
//改 splice
//三个参数，第三个为替换为的内容
let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

schools.splice(2, 0, '江西理工大学');
console.log(schools); //  ["清华大学", "北京大学", "江西理工大学", "浙江大学", "同济大学"]
~~~

~~~js
//查 indexOf()
//第一个参数为寻找的内容，第二个参数为开始的下标，默认为0.返回找到的下标，-1表示未寻得。
let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];

let result = schools.indexOf('浙江大学', 3);
console.log(result); // -1
~~~

-----
-----
<div class="title2">四、函数</div>

-----
<div class="title3">4.1 函数的声明</div>

<div class="title4">使用function声明函数</div>

~~~js
function print(){
    //TODO
}
~~~
<div class="title4">使用函数表达式声明函数</div>

~~~js
let print = function(){
    //TODO
}
~~~
此处为将一个匿名函数赋值给变量
<div class="title4">函数声明的提升</div>

当声明函数时，这段代码会自动提升到头部。所以可以：
> 先使用，再声明

当然，只有使用function声明时才会自动提升，使用函数表达式则不会。
<div class="title4">函数重复声明</div>

后面的会覆盖前面的。
<div class="title4">立即执行函数</div>

>IIFE (immediately invokable function expression)

~~~js
(function() {
  console.log("这个函数只执行一次");
})();
~~~
<div class="title3">4.2 函数参数</div>

~~~js
// 参数 figure(位数) txt(文本) 
function code(figure, txt) {
  const num1 = Math.random() * 0.9 + 0.1;
  const num2 = Math.floor(num1 * Math.pow(10, figure));

  console.log(txt + num2);
}
~~~
> 不用声明类型

<div class="title3">4.2 参数默认值</div>

使用等号给参数默认值
`function code(figure, txt = "随机数：") {/*TODO*/}`
<div class="title3">4.3 计时器</div>

三种延迟法：(`setTimeOut()`)
~~~js
console.log(1);

/**
 * 第一个参数是代码，注意代码需用引号包裹，否则会立即执行代码
 * 第二个参数是 1000，即 1000ms 后执行 console.log(2)
 */
setTimeout('console.log(2)', 1000);

/**
 * 第一个参数是匿名函数
 * 第二个参数是 2000，即 2s 后执行 console.log(3)
 */
setTimeout(function () {
  console.log(3);
}, 2000);

// 第一个参数是函数名，注意函数名后不要加小括号“()”，否则会立即执行 print4
setTimeout(print4, 3000);

console.log(5);

function print4() {
  console.log(4);
}
~~~
我们可以再递归函数中使用计时器，来完成倒计时。
~~~js
// 首先定义计时总秒数，单位 s
let i = 60;

// 定义变量用来储存定时器的编号
let timerId;

// 写一个函数，这个函数即每次要执行的代码，能够完成上述的 1、2、3
function count() {
  console.log(i);
  i--;
  if (i > 0) {
    timerId = setTimeout(count, 1000);
  } else {
    // 清除计时器
    clearTimeout(timerId);
  }
}

// 首次调用该函数，开始第一次计时
count();
~~~

有另一个方法，`setInterval()`,和`setTimeOut()`用法完全一致，但是他是无限次调用。
~~~js
let i = 60;
print();
let timer = setInterval(print, 1000);

function print() {
  console.log(i);
  i--;
  if (i < 1) {
    clearInterval(timer);
  }
}
~~~

-----
-----
<div class="title2">五、对象</div>

-----
> 对象（object）是JavaScript语言的核心概念，也是最重要的数据类型。所有对象都继承自`Object`.

~~~js
let person = {
  name: 'henry',
  age: 18,
  run: function() {
    console.log('running');
  }
}

person.run();
~~~
本质上就是一些键值对。其中键名都是字符串，所以引号可加可不加。

访问所有键：`Object.keys(person)`
~~~js
// 第一步：创建构造函数
function People(name, age) {
  this.name = name;
  this.age = age;
}

// 第二步：通过 new 创建对象实例
let person = new People('henry', 18);
console.log(person);
~~~
构造函数方式创建对象。
<div class="title3">5.1 属性操作</div>

<div class="title4">属性读取</div>

有点运算符和方括号运算符两种方式。
~~~js
console.log(person.name);
console.log(person['name']);
~~~
效果相同。但是后者可以使用变量。  
属性的赋值也可以使用点运算符和方括号运算符。

<div class="title4">属性删除和增加</div>

删除：delete命令。  
增加：直接写进去。
~~~js
let person = {
  name: 'henry',
  age: 18
}

//删除name属性
delete person.name;
//增加gender属性
person.gender = 'male';
~~~
我们可以用in来判断一个对象有无某个属性。`'name' in person;`
或者用Object原型对象自带的方法`hasOwnProperty`
~~~js
person.hasOwnProperty('name');
~~~

<div class="title3">5.2 对象继承</div>

~~~js
// 字面量
let o1 = {
  name: 'alice',
};

// 构造函数
let o2 = new Object();
let o3 = new Object();

// 继承
let o4 = new o1();
~~~
个人理解为，JS的对象和类是混在一起的，既可以做对象，也可以用作JAVA中的类。

<div class="title4">JSON格式转换</div>

~~~js
// 一个 JSON 字符串
const jsonStr =
  '{"sites":[{"name":"Runoob", "url":"www.runoob.com"},{"name":"Google", "url":"www.google.com"},{"name":"Taobao", "url":"www.taobao.com"}]}';

// 转成 JavaScript 对象
const obj = JSON.parse(jsonStr);
//转为JSON
const jsonStr2 = JSON.stringify(obj);
~~~
<div class = "title3">5.3内置对象</div>

<div class = "title4">Math</div>

~~~js
Math.E // 常数e。
Math.LN2 // 2 的自然对数。
Math.LN10 // 10 的自然对数。
Math.LOG2E // 以 2 为底的e的对数。
Math.LOG10E // 以 10 为底的e的对数。
Math.PI // 常数π。
Math.SQRT1_2 // 0.5 的平方根。
Math.SQRT2 // 2 的平方根。

Math.abs() // 绝对值
Math.ceil() // 向上取整
Math.floor() // 向下取整
Math.round() // 四舍五入取整
Math.max() // 最大值
Math.min() // 最小值
Math.pow() // 指数运算
Math.sqrt() // 平方根
Math.log() // 自然对数
Math.exp() // e的指数
Math.random() // 随机数
~~~
<div class = "title4">Storage</div>

该接口用于脚本在浏览器保存数据。两个对象部署了这个接口:`window.sessionStorage`和`window.localStorage`
~~~js
//数据写入，第一个参数为键名key,第二个为键值value
window.localStorage.setItem('myLocalStorage', 'storage Value');
//如果不是字符串类型则先转换为字符串，如下：
const obj = {
  name: 'henry',
  age: 18
}
const value = JSON.stringify(obj);
window.localStorage.setItem('myLocalStorage', value);

//数据读取，接受key，返回value
window.localStorage.getItem('myLocalStorage');

//清除缓存
window.localStorage.clear();
~~~
<div class = "title4">String</div>

>`String`是JS原生提供的三大包装对象之一(另两个是`Number`和`Boolean`)  

`let v2 = new String('abc');`  
包装对象的目的：
1. 使得JavaScript的对象涵盖所有值
2. 使得原始类型的值可以方便地调用某些方法
<div class = "title4">Array</div>

>也是JS的原生对象

~~~js
//连接数组 join()
let arr = [1, 2, 3, 4];
arr.join(' ') // '1 2 3 4'
arr.join(' | ') // "1 | 2 | 3 | 4"
arr.join() // "1,2,3,4"
//和split作用刚好相反

//倒序 reverse()
//略

//排序 sort()
//没有参数就按照字典排序，或者可以传入函数
let arr = [
  { name: 'jenny', age: 18 },
  { name: 'tom', age: 10 },
  { name: 'mary', age: 40 }
];

arr.sort(function(a, b) {
  return a.age - b.age;
});

console.log(arr);
//此处传入的函数，当返回值大于0时，交换前后位置（当前的顺序是错误的）（原先a在前面），小于0时，按照原先的顺序。
~~~
遍历
~~~js
//map
let arr = [
  { name: 'jenny', age: 18 },
  { name: 'tom', age: 10 },
  { name: 'mary', age: 40 }
];

// elem: 数组成员
// index: 成员下标
// a: 整个数组
// 返回由elem.name组成的数组
const handledArr = arr.map(function(elem, index, a) {
  elem.age += 1;
  console.log(elem, index, a);
  return elem.name
});

console.log(arr);
console.log(handledArr);

//forEach
//forEach没有返回值
const handledArr = arr.forEach(function(elem, index, a) {
  elem.age += 1;
  console.log(elem, index, a);
  return elem.name//不会返回。
});
~~~
<div class = "title4">Date</div>

~~~js
//获取当前时间
let now = new Date();
console.log(now);

// 传入表示“年月日时分秒”的数字
let dt1 = new Date(2020, 0, 6, 0, 0, 0);
console.log(dt1);

// 传入日期字符串
let dt2 = new Date('2020-1-6');
console.log(dt2);

// 传入距离国际标准时间的毫秒数
let dt3 = new Date(1578240000000);
console.log(dt3);
~~~
日期运算
~~~js
let dt1 = new Date(2020, 2, 1);
let dt2 = new Date(2020, 3, 1);

// 求差值
let diff = dt2 - dt1;

// 一天的毫秒数
let ms = 24 * 60 * 60 * 1000;

console.log(diff / ms); // 31
~~~
早晚比较
~~~js
let dt1 = new Date(2020, 2, 1);
let dt2 = new Date(2020, 3, 1);

console.log(dt1 > dt2); // false
console.log(dt1 < dt2); // true
~~~
解析日期字符串：`Date.parse()`
~~~js
let dt = Date.parse('2020-1-6');
console.log(dt); // 1578240000000
~~~
时间对象转时间字符串：to方法
~~~js
let dt = new Date();
let dtStr = dt.toJSON();

console.log(dtStr); // 2020-01-03T09:44:18.220Z
~~~
获取事件对象的年月日：get方法
~~~js
let dt = new Date();
dt.getTime(); // 返回实例距离1970年1月1日00:00:00的毫秒数。
dt.getDate(); // 返回实例对象对应每个月的几号（从1开始）。
dt.getDay(); // 返回星期几，星期日为0，星期一为1，以此类推。
dt.getFullYear(); // 返回四位的年份。
dt.getMonth(); // 返回月份（0表示1月，11表示12月）。
dt.getHours(); // 返回小时（0-23）。
dt.getMilliseconds(); // 返回毫秒（0-999）。
dt.getMinutes(); // 返回分钟（0-59）。
dt.getSeconds(); // 返回秒（0-59）。
~~~
以上部分也可通过set方法设置  
<strong>注意月份</strong>

-----