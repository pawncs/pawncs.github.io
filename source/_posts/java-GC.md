---
title: java-GC
date: 2021-03-20 22:16:36
categories:
- Java
tags:
- JVM
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
<div class="title2" >零、目录</div>

-----
   <a href="#java-GC1">一、GC</a>  
   <a href="#java-GC2">二、何时触发GC</a>  
   <a href="#java-GC3">三、如何找到该被清理的对象（存活检测）</a>
   
   3.1 引用计数法   
   3.2 可达性分析法  
   3.3 延伸  
   
   <a href="#java-GC4">四、如何进行清理</a>  
   <a href="#java-GC5">五、垃圾收集器</a>  
   <a href="#java-GCEX">EX、参考</a>  

-----
<div class="title2" id="java-GC1">一、GC</div>

-----
java中，内存的管理：对象的管理，即对象的分配和释放。

在JAVA中，对象的释放，由GC来完成。

-----
<div class="title2" id="java-GC2">二、何时触发GC</div>

-----

[垃圾回收触发条件](https://hadyang.github.io/interview/docs/java/jvm/gc/#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E8%A7%A6%E5%8F%91%E6%9D%A1%E4%BB%B6)

-----
<div class="title2" id="java-GC3">三、如何找到该被清理的对象（存活检测）</div>

-----
GC通常采取有向图的方式（节点为对象，边为引用类型）记录和管理堆（heap）中的所有对象。当其确认一些对象不可达时，GC就回收内存空间。下面除了介绍这种方式（可达性分析）,还介绍了引用计数法。

>判断是否可达->引用计数法、可达性分析法。
<div class="title3">3.1 引用计数法</div>

引用计数法
+ 简单，但速度较慢 
+ 无法判断是否循环利用
+ java未用  
  
>每个对象包含一个引用计数器，当一个句柄同一个对象连接时，加一，超出作用域，或者设为null,减一。垃圾收集器在对象列表中移动巡视，发现引用计数为0，就释放他占据的空间。但这样无法判断循环引用（要找到他们，垃圾收集器要做更多工作）。(参考《JAVA编程思想第四版》P684)


<div class="title3">3.2 可达性分析法</div>

可达性分析法：
>从"GC Root"对象作为起点进行搜索，若其到一个对象间没有可达路径，则称该对象不可达。当一个对象多次（至少2次）被判断为不可达（没有逃脱成为可回收对象的可能性），则真正的回收他。
>>“基于这么一个原理：所有非死锁的对象最终都肯定能回溯到一个句柄，该句柄要么存在堆栈，要么存在静态存储空间。这个回溯链可能会经历许多对象。”——《JAVA编程思想第四版》P684

[GC Root](https://hadyang.github.io/interview/docs/java/jvm/gc/#%E6%A0%B9%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95)

<div class="title3">3.3 延伸-引用</div>

JVM中的引用类型：
+ 强引用
+ 软引用
+ 弱引用
+ 虚引用

最终目标是什么引用类型取决于max(所有链条的min(单链上所有边的强度))
+ 强引用（StrongReference）  
  形如A a = new A();  
  区别：<strong>强引用禁止目标被垃圾收集器收集</strong>

+ 软引用（SoftReference）：  
		内存溢出前清理目标，若和引用队列关联，则加入引用队列。其他情况可以选择清理的时间或是否清楚他们
+ 弱引用（WeakReference）：  
		在GC时会回收所有的weakReference.若和引用队列关联，则加入引用队列	
+ 虚引用（PhantomReference）：  
		<strong>GC时并不会清除虚引用，虚引用必须由程序明确清除</strong>同时也不能通过虚引用取得对象的实例。  
        （虚引用主要用于跟踪垃圾回收的状态。不能单独使用，只能跟队列联合使用）

<div class="title4">参考</div>

[软引用、弱引用、虚引用-他们的特点及应用场景](https://www.jianshu.com/p/825cca41d962)  


[Java中弱引用、软引用、虚引用及强引用的区别](https://www.jianshu.com/p/ade51a91dfd6)

-----
<div class="title2" id="java-GC4">四、如何进行清理</div>

-----

标记清除算法(Mark-Sweep)、复制算法(Copying)、标记整理算法(Mark-Compact)、<strong>分带收集算法（generational collection）</strong>

+ 标记清除：可能产生太多碎片

+ 复制：解决标记清除的缺陷。（实现简单，运行高效，不易产生内存碎片）缺点是只能使用一半的内存。且效率和存活对象数量有关。
+ 标记整理（压缩法）：解决赋值法的缺陷。充分利用空间。标记完后将存活对象向一端移动。

+ <strong>分带收集算法（generational collection）</strong>。根据对象存活的生命周期将内存划分为若干个不同的区域。（老年代和新生代）。老年代每次只有少量回收，新生代每次会有多个回收->新生代使用复制算法，老年代使用标记整理（压缩算法）
（都会要求程序停止运行（一般情况））
    >记忆集


-----
<div class="title2" id="java-GC5">五、垃圾收集器</div>

-----
+ Serial Old收集器（单线程，serial针对新生代，使用复制算法，serial old收集器针对老年代，使用标记整理）（简单高效，带来停顿）

+ ParNew （serial 的多线程版）

+ Parallel Scavenge收集器 （回收时不需要暂停其他用户线程，使用复制算法）（目的是为了达到可控的吞吐量）（没懂什么意思）（吞吐量 = 运行代码/（运行代码+垃圾收集））

+ Parallel Old收集器 （Parallel Scavenge的老年代版本）（多线程和标记整理算法）

+ CMS（Concurrent Mark Sweep） 收集器 （获取最短回收停顿时间为目的的收集器，并发，采用Mark-SWeep算法）(标记清除、运行在老年代)

+ G1收集器面向服务端，充分利用CPU，多核，并发并行，能建立可预测的停顿时间（使命是替换掉CMS）



-----
<div class="title2" id="java-GCEX">EX、参考</div>

-----

+ [Java中的GC简单介绍](https://blog.csdn.net/weixin_44027397/article/details/114383323?spm=1001.2014.3001.5501)

+ [Java 垃圾回收机制与几种垃圾回收算法](https://blog.csdn.net/yrwan95/article/details/82829186?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161490811816780271538995%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=161490811816780271538995&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-2-82829186.pc_search_result_hbase_insert&utm_term=java%E4%B8%89%E7%A7%8D%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E7%AE%97%E6%B3%95)

+ [《剑指Java面试-Offer直通车》--Java底层知识GC](https://blog.csdn.net/lucky_jiexia/article/details/105823438)

+ [深入理解Java虚拟机-垃圾回收器与内存分配策略](https://blog.csdn.net/ThinkWon/article/details/103831676)


-----
