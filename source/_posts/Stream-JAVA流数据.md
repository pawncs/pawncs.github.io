---
title: Stream--JAVA流数据
date: 2021-03-06 12:27:49
categories:
- Java
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
<div class="title2">一、简介</div>

-----
+ 主要目的是提高开发效率，使代码干净整洁
+ 主要作用对象是集合（Collection）
+ 常与Lambda配合使用

-----
<div class="title2">二、创建流&迭代流</div>

-----

<div class="title4">创建流</div>

~~~java
//Stream<T> stream =  Stream.of([数组或者直接一个个的对象]),下面是一个个对象的例子
Stream<String> stream = Stream.of("a", "b", "c", "d", "e");
//实现Collection的类都有转换为Stream的方法
Stream<String> stream = list.stream();
~~~
<div class="title4">迭代流</div>

~~~java
stream.forEach(System.out::println);
~~~
-----
<div class="title2">三、流过滤</div>

-----

~~~java
//通过filter函数，留下来的都是条件表达式为true的。
list.stream()
    .filter(f -> [条件表达式])
    .forEach(f -> {System.out.println(f.getName());});
~~~

-----
<div class="title2">四、流数据映射</div>

-----

+ 用映射的对象替换源对象。（类型可以不一致）（更灵活）  
  `stream.map(f ->{return f+1})`

-----
<div class="title2">五、流数据排序</div>

-----

+ 和Collection实现类的sort差不多
  `students.stream()
    // 实现升序排序
    .sorted((student1, student2) -> {
        return student1.getRollNo() - student2.getRollNo();
    }).forEach([其他])`

-----
<div class="title2">五、流数据摘取</div>

-----

+ `limit`函数，可摘取流的前n个（limit的参数）

-----
<div class="title2">六、流合并</div>

-----

+ 目的是将流的所有内容合并到一个对象  
  `stream.reduce((a,b)->a + b).get()`
  + a是流的第一个对象，b是第二个，运算结果储存到a，b是第三个（以此类推），直到合并完成
  + 也可以新建对象作为存储，这样就不会将流破坏  
    `stream.reduce(new T(),(a,b)->{return a;});`此时a表示缓存的元素，b表示从第一个开始的每一个对象。
    + 此时因新建了缓存对象，因此不用再调用`get()`方法

-----
<div class="title2">七、流收集</div>

-----
+ 和`forEach`和`reduce`一样，也是终点方法。
+ 目的是将流重新存储至List等里。
  `stream.collect(Collectors.toList())`
  >这里注意`java.util.stream.Collectors`，是stream工具包提供的收集器

-----
<div class="title2">流的设计思想</div>

-----

+ 管道，每个点完成一个操作  
  `.filter().forEach()`
+ 编程的重点不是对象的运用而是数据的运算，更加简洁，清晰
  + 特征是函数式风格。
-----
<div class="title2">EX、并行流</div>

-----
+ 管道的执行方式显然是串行的。这种模式无法发挥多核CPU的优势。
+ 因此，我们需要把串行的计算模式改为并行的计算方式。
  + 只需要把`stream()`方法改为`parallelStram()`方法即可使用并行流。（其他方法相同）
+ 当每个元素间有逻辑依赖时不适合使用并行流。（因为他会打乱顺序）
+ 当硬件不行的时候并行效果不一定好（吃线程数）
+ 当任务简单的时候并行效果不一定好（分配的过程的开销更大）
-----