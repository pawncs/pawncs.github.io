---
title: java_io
date: 2021-04-07 21:31:03
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

<a href="#java-io-1">一、文件的编码</a>  
<a href="#java-io-2">二、File类的使用</a>  
<a href="#java-io-3">三、RandomAccessFile 基本操作</a>  
<a href="#java-io-4">四、字节流</a>  
<a href="#java-io-4-1">4.1 文件输入流FileInputStream</a>  
<a href="#java-io-4-2">4.2 文件输出流FileOutputStream</a>  
<a href="#java-io-4-3">4.3 数据输入输出流</a>  
<a href="#java-io-4-4">4.4 字节缓冲流</a>  
<a href="#java-io-5">五、字符流</a>  
<a href="#java-io-6">六、对象的序列化和反序列化</a>  
<a href="#java-io-6-1">6.1 序列化基本操作</a>  
<a href="#java-io-6-2">6.2 transient及ArrayList源码分析</a>  
<a href="#java-io-6-3">6.3 序列化中子父类构造函数问题</a>  
<a href="#java-io-ex">EX、其他</a>  


-----
<div class="title2" id="java-io-1">一、文件的编码</div>

-----
[字符集与编码](https://pawncs.github.io/2021/03/20/%E5%AD%97%E7%AC%A6%E9%9B%86%E4%B8%8E%E7%BC%96%E7%A0%81/)

`s.getBytes("[指定的类型]")`得到字节序列`Byte[]`，若不指定，则获得项目默认编码

+ 补充GBK编码：中2英1(gbk是gb2312的扩充，加了更多的汉字)

+ JAVA是双字节编码UTF-16be（中英均为2字节）
+ (be:big endian：大端：高位字节放低地址)

>乱码：当把字节序列变成字符串时，需要使用[项目]同样的编码方式解析，不然会出现乱码。  
`String a = new String(bytes,'utf-16be')`

-----
-----
<div class="title2" id="java-io-2">二、File类的使用</div>

-----
<div class="title3" id="java-io-2-1">2.1 File类 常用API</div>

`java.io.File`可操作硬盘的文件（目录）

>File类只用于表达文件信息（名称，大小），不能访问文件内容
`File file = new File("D:\\[路径]")`
>+ 双斜杠（转义字符）或者反斜杠。
>+ <strong>或者可以`File("D:"+File.separator+"[路径]")`来使之可以在任何系统下添加分隔符。</strong>

+ `file.exists()` 判断文件是否存在
  
+ `file.mkdir()` （不存在的话可以）创建目录（`mkdirs()`可以创建多级目录）;`file.createNewFile()`创建文件
>个人感觉file对象似乎只是一个指向指定地址的引用一样的东西，所以可以新建立File对象，再去看他是否存在以及创建一个文件或目录
+ `file.delete()`删除目录
  
+ 是否是一个目录`isDirectory()`、是否是一个文件`isFile()`

+ file的`toString`是他的路径。
+ `getParent()`,`getName()`(文件名)，`getAbsolutePath()`……
+ <strong>遍历</strong>：`String[]::list()`,`File[]:: listFiles()`

-----
-----
<div class="title2" id="java-io-3">三、RandomAccessFile 基本操作</div>

-----


+ `RandomAccessFile`提供了对文件内容的访问(读写)（`File`只用于表示文件或目录基本信息，不能用于访问内容）
+ `RandomAccessFile`支持随机访问文件（访问任意位置）

~~

+ java文件模型：硬盘上的文件是按byte存储的，是数据的集合
+ 打开文件：两种模式："rw","r"。（读写，只读）。

+ 构造：`new RandomAccessFile(file,"rw")`.打开后有个指针`pointer`。初始为0表示在开头。

~~

+ 写方法`write(int)`。（只写一个字节（后八位）），同时指针后移。
  ~~~java
  //写一个整数
  int i = 0x7fffffff;//四字节，32位,所以写四次。
  raf.write(i>>>24);//留下了高8位
  raf.write(i>>>16);
  raf.write(i>>>8);
  raf.write(i);

  //writeInt等其他方法……
  ~~~
+ 读方法`read()`(读1字节)
~~~java
raf.seek(0);//移动指针到开头
raf.read(buf);//参数为byte[],则直接读完。
~~~
+ 文件读完后一定要关闭。

+ 其他操作可查询java手册(javadoc)。

-----
-----
<div class="title2" id="java-io-4">四、字节流</div>

-----
>IO流：输入+输出  
也分为字节流和字符流

> 这一章讲字节流，他有两个抽象父类（input,output）
<div class="title3" id="java-io-4-1">4.1 文件输入流FileInputStream</div>

>EOF = End 读到-1就到结尾

InputStream的抽象方法
+ `read()` 读一个字节无符号的填充到低八位
+ `read(byte[] buf)`读取到的直接填充到数组中
+ `read(byte[] buf，int start,int size)`读取到的直接填充到数组的start上，存放size长度。（返回读到的长度）（读取失败返回-1）
~~~java
    //典型写法
    byte[] buf = new byte[8*1024];//[申请空间]
    int bytes = 0;
    while(bytes = in.read(buf,0,buf.length) != -1){
        //操作buf
    }//重复利用buf,完成输入操作
~~~
+ 输出流与之对应（`write()`方法）

> `Integer.toHexString(buf[i] & 0xff)`这里`&0xff`是为了将高位清零防止出现数据转换错误。

<div class="title3" id="java-io-4-2">4.2 文件输出流FileOutputStream</div>

继承了OutputStream,实现了向文件中写出byte数据的方法。

构造函数里可确定是否是“追加”

<div class="title3" id="java-io-4-3">4.3 数据输入输出流</div>

>DataInputStream,DataOutputStream 对流功能的扩展，可以更方便的读取int long 字符等类型数据

`writeInt writeDouble writeUTF`等，把本来写多次的操作包装好。

<div class="title3" id="java-io-4-4">4.3 字节缓冲流</div>

>BufferedInputStream BufferedOutputStream,
为io提供了带缓冲区的操作，提高了IO的性能

`bos.flush()` 刷新缓冲区

-----
-----
<div class="title2" id="java-io-5">五、字符流</div>

-----
+ 注意编码问题
+ 认识文本和文本文件
  + java文本（char）是16位无符号整数，是字符的unicode编码（双字节）。文件是byte byte 的数据序列。
  + 文本文件是文本(char)序列按照某编码方案序列化为byte的存储结果。
+ 字符流（Reader Writer）,一次处理一个字符。底层仍然是字节序列。-->操作的是文本文件
  + 字符流基本实现
    + `InputStreamReader`：byte流->char流,按照编码解析
    + `outputStreamWriter`:char流->byte流，按照编码处理

+ 字符流过滤器（缓冲？）
  + BufferedReader/BufferedWriter。好处是通过嵌套可以设置编码，（FileReader等不行）同时提供一次读一行等的功能
  + PrintReader等。
  
-----
-----
<div class="title2" id="java-io-6">六、对象的序列化和反序列化</div>

-----

>序列化和反序列化，就是Object和byte序列的互相转换。

<div class="title3" id="java-io-6-1">6.1 序列化基本操作</div>

+ 序列化流`ObjectOutputStream`，是过滤流。核心方法`writeObject`
+ 反序列化流`ObjectinputStream`，核心方法`readObject`
+ 序列化接口（`Serializable`）对象必须实现序列化接口才能序列化。（否则将异常）


<div class="title3" id="java-io-6-2">6.2 transient及ArrayList源码分析</div>

+ `transient`关键字：加了后不会进行JVM默认的序列化工作。（可以自己完成序列化）
~~~java
//自己进行序列化
//在要序列化的方法中加入
private void writeObject(java.io.ObjectOutputSream s) throws java.io.IOException{
    s.defaultWriteObject();//完成jvm默认的序列化操作
    s.writeInt([自己要完成的序列化属性]);
    //……
}
private void readObject(java.io.ObjectOutputSream s) throws java.io.IOException,ClassNotFoundException{
    s.defaultReadObject();//完成jvm默认的反序列化操作
    this.[自己要完成反序列化的属性] = s.readInt();
    //……
}
~~~
需要注意的是，序列化和反序列化操作元素的顺序需要一致。

+ 自己写序列化可以提高性能。如ArrayList中自己完成了数组元素的序列化。——如此可以有效元素序列化而不是全部。

[transient关键字的作用，writeObject和readObject方法的作用](https://blog.csdn.net/csndhu/article/details/102681015)  
[ArrayList底层代码中的writeObject和readObject问题思考](https://www.cnblogs.com/homesea/articles/10203270.html)

<div class="title3" id="java-io-6-3">6.3 序列化中子父类构造函数问题</div>

>对子类进行反序列化操作时，若父类没有实现序列化接口，其构造函数会被显示调用

-----
-----
<div class="title2" id="java-io-ex">EX、其他</div>

-----
[学习视频](https://www.imooc.com/video/1832)

基本操作可查询`jdk api 1.8_google.CHM` (javadoc)

-----