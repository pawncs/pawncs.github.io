---
title: java_io_2
date: 2021-07-19 17:15:41
categories:
- Java
tags:
- IO
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

<a href="#java_io_2-1">一、基础概念</a>  
<a href="#java_io_2-2">二、LINUX IO</a>  
<a href="#java_io_2-2-1">2.1 概念说明</a>  
<a href="#java_io_2-2-2">2.2 IO模式</a>  
<a href="#java_io_2-ex">EX、参考</a>  

-----
<div id="java_io_2-1" class="title2">一、基础概念</div>

-----
<div class="title4">1.1 同步、异步、阻塞、非阻塞</div>

+ 同步（Synchronous）  
+ 异步( Asynchronous)


+ 阻塞( Blocking )
+ 非阻塞( Nonblocking)

<div class="title4">1.2 区别</div>

需要关注是进程层次还是IO系统层次。

-----
-----
<div id="java_io_2-2" class="title2">二、LINUX IO</div>

-----
<div id="java_io_2-2-1" class="title3">2.1 概念说明</div>
<div class="title4">2.1.1 用户空间与内核空间</div>

用户不能直接访问内核；虚拟空间前3G为用户空间，最高1G字节为内核空间供内核使用。

<div class="title4">2.1.2 进程切换</div>

从一个进程切换到另一个进程过程：
1. 保存处理机上下文，包括程序计数器和其他寄存器
2. 更新PCB信息
3. 把进程的PCB移入响应的队列，如就绪，某事件的阻塞等队列
4. 更新内存管理的数据结构
5. 恢复处理机上下文

>PCB:进程控制块(Process Control Block),用于描述进程

<div class="title4">2.1.3 进程阻塞</div>

当进程期待的某些事件未发生，则系统自动执行阻塞原语(Block)，使自己由运动状态变为阻塞状态。
>进程进入阻塞状态是不占用CPU资源的。

<div class="title4">2.1.4 文件描述符fd</div>

fd:File descriptor,表示指向文件的引用的抽象化概念。其在形式上是一个非负整数，为一个索引值，指向文件记录表。  
当打开现有文件或创建新文件时，内核向进程返回一个文件描述。

<div class="title4">2.1.5 缓存I/O</div>

缓存IO又称标准IO。为大多数文件系统的默认IO。在LINUX的IO机制中，操作系统会将IO的数据缓存在文件系统的页缓存中。即：数据会先被拷贝到操作系统内核的缓冲区中，然后再从内核缓冲区拷贝到应用程序的地址空间。

<div id="java_io_2-2-2" class="title3">2.2 IO模式</div>

对一次IO访问（read举例），有两个阶段：
+ 等待数据准备
+ 将数据从内核拷贝到进程中

针对两个阶段如何处理，产生了5种网络模式。

<div class="title4">2.2.1 阻塞I/O(blocking IO)</div>

默认。  
特点：IO执行的两个阶段都被阻塞了。

~~~
recvfrom -------------->  no datagram ready
           system call           |
                                 |
                                 |
                                 |
                                 |
                                 ▽
                           datagram ready
                           copy dataffram
                                 |
                                 |
                                 |
                                 |
                                 ▽
process <---------------   copy complete
datagram     return OK
~~~

<div class="title4">2.2.2 非阻塞I/O(nonblocking IO)</div>

操作时，若数据未准备好，则不会阻塞进程，而是直接返回error。数据拷贝到用户进程时仍然会阻塞至完成。  
特点：用户需不断主动询问核心数据是否准备完成。
~~~
recvfrom -------------->  no datagram ready
           system call           
         <--------------
           BWOULDBLOCK
recvfrom -------------->  no datagram ready
           system call           
                           datagram ready
                           copy dataffram
                                 |
                                 |
                                 |
                                 |
                                 ▽
process <---------------   copy complete
datagram     return OK
~~~

<div class="title4">2.2.3 I/O多路复用(IO multiplexing)</div>

select,poll,epoll的方法会不断轮询所有socket.当某个socket有数据到达了，就通知用户进程.用户进程再去执行read操作，将数据从核心拷贝到用户进程。两个阶段用户进程会分别被阻塞。  
特点：单个进程能同时处理多个网络连接的IO

~~~
select   -------------->  no datagram ready
           system call           |
                                 |
                                 |
                                 |
                                 |
                                 ▽
         <--------------  datagram ready
         return readable
recvfrom -------------->   copy dataffram
            system call          |
                                 |
                                 |
                                 |
                                 |
                                 ▽
process <---------------   copy complete
datagram     return OK
~~~

<div class="title4">2.2.4 信号驱动I/O(signal driven IO)</div>

>并不常用

<div class="title4">2.2.5 异步I/O(asynchronous IO)</div>

用户进程发起read后即可去做其他事。
核心角度，当他收到请求后，先返回用于避免用户进程的阻塞。而后，等数据准备完成，则拷贝到用户内存，完成后，发送signal给用户进程，表示读取完成。

~~~
aio_read -------------->  no datagram ready
           system call           |
         <--------------         |
             return              |
                                 |
                                 |
                                 |
                                 ▽
                           datagram ready
                           copy dataffram
                                 |
                                 |
                                 |
                                 |
                                 |
                                 |
signal                           |
handler                          ▽
process <---------------   copy complete
datagram     return OK
~~~
-----
-----
<div id="java_io_2-ex" class="title2">EX、参考</div>

-----
[BIO，NIO，AIO概览](https://juejin.cn/post/6844903664256024584)  
[同步/异步，阻塞/非阻塞概念深度解析](https://blog.csdn.net/lengxiao1993/article/details/78154467)  
[网络IO模型](https://blog.csdn.net/zhoudaxia/article/details/8974779?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)  
[JAVA五种IO模型](https://blog.csdn.net/weixin_44579258/article/details/90758359?utm_medium=distribute.pc_relevant_download.none-task-blog-2~default~BlogCommendFromBaidu~default-2.test_version_3&depth_1-utm_source=distribute.pc_relevant_download.none-task-blog-2~default~BlogCommendFromBaidu~default-2.test_version_)  
[*Linux IO模式及 select、poll、epoll详解](https://segmentfault.com/a/1190000003063859)

-----
