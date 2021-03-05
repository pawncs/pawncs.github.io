---
title: Lambda表达式
date: 2021-03-05 23:17:35
categories:
- Java
tags:
- Lambda
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
<div class="title2">一、无类型参数</div>

-----
<div class="title4">基本定义</div>

+ 表达式的基本结构：`f->{}`  
    在功能上等同于匿名方法
    ~~~java
    public void unknown(f) {
        System.out.println(f.getName());
    }
    ~~~
    
<div class="title4">类型识别</div>

+ f的变量类型是上下文自动识别的。
+ 必须配合上下文和其他方法使用，不能独立使用。
<div class="title4">多参数/无参数</div>

+ 箭头前表示参数变量：`(a,b)->{}`
+ 无参数：`()->{}`

> 当执行语句只有一条时，不需要加大括号（大括号本身只是划分块）

~~~java
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(3);
        list.add(2);
        list.sort((a, b) -> {
            return a.compareTo(b);
        });
~~~
-----
<div class="title2">二、有类型参数</div>

-----
其实就是为了代码易阅读。
`(Type a)->{}`.其他跟无类型一样。

-----
<div class="title2">三、引用外部变量&规范</div>

-----
+ 规范一：引用的外部变量不能被修改。（最终变量或实际上的最终变量）(final)。（表达式前允许一次赋值）（Lambda表达式内或者后面不允许修改）
+ 规范二：参数不能和局部变量同名。
  ~~~java
    String s = "aaa";
    list.forEach(s ->{});
    //参数同名，是错误的
  ~~~
-----
-----
<div class="title2">四、双冒号操作符</div>

-----
`System.out::println`  
即：  
`n -> {System.out.println(n);}`  

+ `A::B`   
  将参数传递给A类（或A对象）的B方法。（省略了参数）

+ 用法一.调用静态或非静态方法：
    `fruits.forEach(System.out::println)`
+ 用法二.多参数：系统会自动获取上下文参数，并按顺序传给指定方法。
  ~~~java
    //定义函数
  private static int compute(Student s1, Student s2) {
  ... ...
  ... ...
    }
    //排序就可以写为
    Collections.sort(students, SortTest::compute);
  ~~~
+ 用法三.调用父类方法:
  `list.forEach(super::print);`
-----