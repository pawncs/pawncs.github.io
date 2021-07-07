---
title: http协议
date: 2021-05-21 16:02:36
categories:
- network
tags:
- dictionary
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

-----
<a href="#http-1">一、web和网络基础</a>  
<a href="#http-1-1">1.1 http版本区别</a>  
<a href="#http-1-2">1.2 tcp/ip协议族</a>  
<a href="#http-1-3">1.3 与http密切相关的ip,tcp,dns</a>  
<a href="#http-1-4">1.4 url,uri</a>  

<a href="#http-2">二、简单的http协议</a>  
<a href="#http-2-1">2.1 请求报文、响应报文</a>  
<a href="#http-2-2">2.2 请求方法</a>  
<a href="#http-2-3">2.3 持久连接</a>  

<a href="#http-3">三、http报文内的http信息（未完成）</a>  
<a href="#http-3-1">3.1 首部字段</a>  
<a href="#http-3-2">3.2 压缩传输编码、分块传输编码</a>  
<a href="#http-3-3">3.3 发送多种数据的多部分对象集合</a>  
<a href="#http-3-1">3.4 范围请求</a>  
<a href="#http-3-2">3.5 内容协商</a>  

<a href="#http-4">四、http状态码</a>  
<a href="#http-4-1">4.1 代表性状态码</a>  
<a href="#http-4-2">4.2 状态码详解</a>  

<a href="#http-5">五、与http协作的web服务器</a>  
<a href="#http-5-1">5.1 单台虚拟主机实现多个域名</a>  
<a href="#http-5-2">5.2 通信数据转发程序：代理、网关、隧道</a>  

<a href="#http-6">六、http首部</a>  
<a href="#http-6-1">6.1 http首部字段概况</a>  
<a href="#http-6-2">6.2 http首部字段详细</a>  

<a href="#http-7">七、https</a>  

<a href="#http-8">八、用户身份认证</a>    
<a href ="#http-8-1" >8.1 BASIC 认证</a>  
<a href ="#http-8-2">8.2 DIGEST 认证</a>  
<a href ="#http-8-3">8.3 SSL 客户端认证</a>  
<a href ="#http-8-4">8.4 基于表单认证</a>   

<a href="#http-9">九、基于http的功能追加协议</a>  

<a href="#http-10">十、构建web内容的技术</a>  

<a href="#http-11">十一、web的攻击技术</a>  
<a href ="#http-11-1" >11.1 因输出值转义不完全引发的安全漏洞</a>  
<a href ="#http-11-2" >11.2 因设置或设计上的缺陷引发的安全漏洞</a>  
<a href ="#http-11-3" >11.3 因会话管理疏忽引发的安全漏洞</a>  
<a href ="#http-11-4" >11.4 其它安全漏洞</a>  


<a href="#http-ex">EX、额外</a>

-----

-----
<div id ="http-1" class="title2">一、web和网络基础</div>

-----
>http(HyperText Transfer Protocol):超文本传输协议

web使用其作为规范，完成从客户端到服务端等一系列运作过程。

<div id="http-1-1" class="title3">1.1 http版本区别</div>

+ http0.9：正式版本之前的版本的意思
+ http1.0：正式版本
+ http1.1：主流版本（<a href="#http-2-3">持久连接</a>）
+ http2.0：升级版（<a href="#http-9">改善速度体验</a>）
<div id="http-1-2" class="title3">1.2 TCP/IP协议族</div>

> 具体可阅读《详解TCP/IP》  

分层：
+ 应用层
  + 其决定了向用户提供应用服务时通信的活动
  + FTP（file transfer protocol,文件传输协议）,DNS（domain name system,域名系统）,HTTP
+ 传输层
  + 其为上层提供处于网络连接中的两台计算机之间的数据传输
  + TCP（transmission control protocol，传输控制协议）,UDP（user data protocol，用户数据报协议）
+ 网络层
  + 其用于处理网络上流动的数据包，该层规定了通过了路径到达对方的计算机（选择路径）
+ 链路层
  + 用于处理连接网络的硬件部分，包括控制操作系统、设备驱动、NIC（网卡），光纤

<div id="http-1-3" class="title3">1.3 与HTTP密切相关的IP,TCP,DNS</div>

<div class="title4">IP</div>

其作用为把数据包传送给对方。为此需要得知IP地址和MAC地址。IP地址指明了节点被分配到的地址，MAC指明了网卡所知的固定地址。

+ IP间的通信依赖MAC地址，通常通过ARP协议来搜寻中转目标。（ARP协议根据通信方IP地址反查对应的MAC地址）
+ 没人能全面掌握互联网中的传输状况；路由选择机制。
<div class="title4">TCP</div>

三次握手，四次挥手。SYN，ACK。

TCP确保可靠性。
<div class="title4">DNS</div>

负责域名解析。提供了域名到IP地址的解析服务。

<div id="http-1-4" class="title3">1.4 URL,URI</div>

> url(uniform resource locator:统一资源定位符)  
> uri(uniform resource identifier:统一资源标识符)

uri用字符串标识某一互联网资源，url表示资源的地点。url是uri的子集。

~~~
RFC3986：URI例子

ftp://ftp.xx.xx.xx/xxx/xxxx.txt
http://www.xxx.xx/xxx/xxx.txt
tel:+1-816-555-1212
telnet://192.9.2.16:80/
……
等。
~~~
绝对URI格式：
[协议方案名]://[登录信息]@[服务器地址]:[服务器端口号]/[带层次的文件路径]?[查询字符串]#[片段标识符]

+ 协议方案名也可为数据方案名或者脚本方案名（data:,javascript:）

-----
-----
<div id ="http-2" class="title2">二、简单的http协议</div>

-----
<div id ="http-2-1" class="title3">2.1 请求报文、响应报文</div>

+ 请求报文：  
  请求方法(get)、请求URI、协议版本(http/1.1)、请求首部字段(request field)、内容实体
  ~~~http
  POST /index HTTP/1.1
  Host: xxxx.xx
  Connection: keep-alive
  Content-type: application/x-www-form-urlencoded
  Content-Length: 16

  name=ueno&age=37
  ~~~
  > 请求头和内容实体由空行分隔  

+ 响应报文:  
  服务器对应HTTP版本（http/1.1）、状态码和原因短语(200 OK)，首部字段(head field)，空行分隔，资源实体的主体
  ~~~http
  Http/1.1 200 OK
  Date: Tue,10 Jul 2012 06:50:15 GMT
  Content-Length: 362
  Content-Type: text/html

  <html>
  ……
  </html>
  ~~~
<div id ="http-2-2" class="title3">2.2 请求方法</div>

+ GET：获取资源  
  + 返回URI对应的资源
  
+ POST：传输实体主体  
  + 虽然可以用GET方法传输，但一般不那么做  
  + POST的主要目的不是为了获得响应的主体内容
+ PUT：传输文件  
  + 如同FTP协议的文件上传，其要求报文的主体包含文件内容，保存到请求URI指定的位置
  + 由于HTTP/1.1自身不带验证机制，所以WEB网站一般不使用该方法，若配合web应用程序验证机制或架构为REST等，则可能开放PUT
  
+ HEAD：获得报文首部  
  + 和GET相同，但不返回报文主体。用于确认URI有效性及资源更新的日期时间等。
+ DELETE：删除文件  
  + 用法和PUT相反。
  + 和PUT一样，由于http/1.1自身不带验证机制，因此一般也不使用，若配合web应用程序的验证机制，或遵守REST标准还是可能开放使用的
+ OPTIONS：询问支持的方法  
  + 用于查询针对请求URI指定的资源支持的方法
+ TRACE：追踪路径  
  + 让web服务器将之前的请求通信返回给客户端
  + 具体流程：在Max-forwards首部字段填入数值，每过一个服务器端就将该数字减1，数值为0时停止传输，最后接收到请求的服务器端返回状态码200 OK的响应
  + 客户端通过Trace方法可以查询发送出去的请求是如何被加工、篡改的。TRACE方法用于确认连接过程中发生的一系列操作。
  + TRACE方法本身不常用，加上容易引起XST攻击（Cross-site tracing，跨站追踪），就更不会用到了。
+ CONNECT：要求用隧道协议连接代理
  + 要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。
  + 主要使用 SSL和TLS协议把通信内容加密后经网络隧道传输。
  + 格式：`CONNECT 代理服务器名：端口号 HTTP版本`
  + <a href="#http-5-2">隧道</a>


<div id ="http-2-3" class="title3">2.3 持久连接</div>

http初始版本：
~~~
建立TCP连接
HTTP请求、响应（获取html文档）
断开TCP连接

建立TCP连接
HTTP请求、响应（获取图片）
断开TCP连接

建立TCP连接
HTTP请求、响应（获取图片）
断开TCP连接
~~~

http/1.1和一部分1.0提出了持久连接（http keep-alive）（任意一方未提出断开连接则一直保持连接）  
+ 持久连接：
~~~
-------SYN-------->   ╕
<----SYN/ACK-------   ╞ 建立TCP连接
-------ACK-------->   ╛
-----HTTP请求----->
<----HTTP响应------
-----HTTP请求----->
<----HTTP响应------
<-------FIN--------   ╗
--------ACK------->   ╠ 断开TCP连接
--------FIN------->   ║
<-------ACK--------   ╝
~~~
+ 管线化
~~~
<---建立TCP连接--->
-----HTTP请求----->
-----HTTP请求----->
<----HTTP响应------
<----HTTP响应------
<---断开TCP连接--->
~~~
> 持久连接使得管线化（pipelining）成为可能。管线化：不用等待响应即可发下个请求
-----
-----
<div id ="http-3" class="title2">三、http报文内的http信息</div>

-----

~~~
http报文结构：
[  报文首部 ]
[空行(CR+LF)]
[  报文主体 ]
~~~

<div class="title3" id="http-3-1">3.1 首部字段</div>

+ 请求/响应首部字段
+ 通用首部字段
+ 实体首部字段

<div class="title3" id="http-3-2">3.2 压缩传输编码、分块传输编码</div>
<div class="title3" id="http-3-3">3.3 发送多种数据的多部分对象集合</div>
<div class="title3" id="http-3-4">3.4 范围请求</div>
<div class="title3" id="http-3-5">3.5 内容协商</div>

-----
-----
<div id ="http-4" class="title2">四、http状态码</div>

-----

<table>
<tr>
<th></th>
<th>类别</th>
<th>原因短语</th>
</tr>
<tr>
<td>1XX</td>
<td>Informational(信息性状态码)</td>
<td>接收的请求正在处理</td>
</tr>
<tr>
<td>2XX</td>
<td>Success(成功状态码)</td>
<td>请求正常处理完毕</td>
</tr>
<tr>
<td>3XX</td>
<td>Redirection(重定向状态码)</td>
<td>需要进行附加操作完成请求</td>
</tr>
<tr>
<td>4XX</td>
<td>Client Error(客户端错误状态码)</td>
<td>服务器无法处理请求(客户端出错)</td>
</tr>
<tr>
<td>5XX</td>
<td>Server Error(服务端错误状态码)</td>
<td>服务器请求出错</td>
</tr>
</table>

<div class="title3" id="http-4-1">4.1代表性状态码</div>
+ 200 OK
  + 请求已正常处理
  
+ 204 No Content
  + 请求处理成功，但返回的响应报文不含主体
+ 206 Partial Content
  + 该状态码表示客户端进行了范围请求（只要一部分），服务器成功执行。
  + 响应报文Content-Range 指定报文实体内容
+ 301 Moved Permanently
  + 永久性重定向。
  + 表示请求的资源已经被分配了新的URI，以后应用现在的URI（应按Location的首部重新保存URI）
+ 302 Found
  + 临时性重定向。
  + 表示希望用户（本次）使用新的URI访问（需要保留原先的URI）
+ 303 See Other
  + 该请求对应的资源存在另一个URI，应使用GET定向获取请求的资源
  + 和302类似，但要求POST换GET
+ 304 Not Modified
  + 服务端接受客户端的请求，但发生了请求的条件未满足的情况（If-Match，If-Range等首部）将直接返回304（不包含响应主体）
  + 虽然在3XX，但和重定向无关
+ 307 Temporary Redirect
  + 临时重定向
  + 和302相同。
+ 400 Bad Request
  + 请求报文中存在语法错误
  + 需修改请求内容后重发请求
  + 浏览器会像对待200一样对待400
+ 401 Unauthorized
  + 请求需要有通过HTTP认证的认证信息。
  + 返回401的响应必须由一个适用于被请求资源的WWW-Authenticate首部用以质询（challenge）用户信息。当浏览器初次接收401时，会弹出认证用的对话窗口。
  + 若之前有过1次请求，则表明认证失败。
+ 403 Forbidden
  + 对请求资源的访问被服务器拒绝
  + 服务器可能在响应主体部分给出说明
+ 404 Not Found
  + 服务器没有请求的资源
+ 500 Internal Server Error
  + 服务器在执行请求时发生了错误，也可能是Web应用存在的BUG或某些临时的故障
+ 503 Service Unavailable
  + 服务器正处于超负载或正在进行停机维护，无法处理请求
  + 如果事先知道解除以上状况时间，可写入Retry-After首部字段再发回客户端


> 不少情况状态码是错误的，例如Web应用程序内部发生错误，可能仍会返回200
<div class="title3" id="http-4-2">4.2 状态码详解</div>

[http状态码详解](https://tool.oschina.net/commons?type=5)

-----
-----
<div id ="http-5" class="title2">五、与http协作的web服务器</div>

-----
<div id ="http-5-1" class="title3">5.1 单台虚拟主机实现多个域名</div>

+ 虚拟主机实现多个域名，host的作用
  > 因为DNS服务已将域名映射到ip地址，因此请求到达服务器时已经是ip地址形式了。因此服务器若托管了两个域名，需要先弄清楚是哪个域名。所以在http请求的Host首部需指定主机名或域名的URI
<div id ="http-5-2" class="title3">5.2 通信数据转发程序：代理、网关、隧道</div>

这些应用程序和服务器可以将请求转发给通信线路的下一站服务器，并且能接受到从那台服务器发送的响应并再转发给客户端
+ 代理
  + 扮演了中间人的角色，将一方的请求/响应转发给另一方
  + 可级联多台代理服务器，转发时，需用Via首部字段标记出经过的主机信息。
  + 代理目的：缓存技术减少流量，组织内部针对特定网站的访问控制，获取访问日志等
  + 缓存代理
    + 代理转发响应时，缓存代理（Caching Proxy）会预先将资源的副本保存在代理服务器，当代理再次接收相同的请求时，就不会再次访问源服务器，而是直接返回缓存的资源。
      + 浏览器也可储存缓存
    + 优势：源服务器不必多次处理相同的请求，客户端可以就近从缓存服务器获取资源
    + 缓存期限问题，需要向源服务器确认是否为“旧资源”
  + 透明代理
    + 转发请求或响应时，不对报文做任何加工。反之，则为不透明代理
  + 正向代理（隐藏客户端）
  + 反向代理（隐藏服务器）

+ 网关
  + 网关是转发其他服务器通信数据的服务器
    + 接收请求时，向自己拥有源服务器资源一样对请求进行处理，客户端甚至不会察觉自己通信目标是网关
  + 网关的工作原理和代理相似，而网关能使通信线路上的服务器提供非http协议服务
    + 利用网关可以由HTTP请求转化为其他协议通信
  + 网关能提高通信安全性，因为可以在客户端和网关之间的通信线路上加密以确保连接安全
  + 网关可以连接数据库，使用sql查询数据，也可与信用卡结算系统联动
+ 隧道
  + 隧道是在相隔甚远的客户端和服务器两者间进行周转，并保持双方通信连接的应用程序。
  + 隧道可按要求建立一条与其他服务器通信的线路，届时使用SSL等加密手段进行通信。
  + 隧道的目的是确保客户端和服务器进行安全的通信
  + 隧道本身不会解析HTTP请求

-----
-----
<div id ="http-6" class="title2">六、http首部</div>

-----


<div id ="http-6-1" class="title3">6.1 http首部字段概况</div>

<div class="title4">1.Http请求报文：</div>

+ 报文首部
  + 请求行
    + 方法、URL、HTTP版本
  + 请求首部字段
  + 通用首部字段
  + 实体首部字段
+ 空行（CR+LF）
+ 报文主体

<div class="title4">2.HTTP响应报文：</div>

+ 报文首部
  + 状态行
    + HTTP版本、状态码
  + 响应首部字段
  + 通用首部字段
  + 实体首部字段
+ 空行（CR+LF）
+ 报文主体

<div class="title4">3.若首部字段重复：</div>

规范内尚未明确，不同浏览器处理结果不一致。有的浏览器优先第一次出现的，有的浏览器优先最后出现的

<div class="title4">4.首部字段类型</div>

+ 通用首部字段(General Header Fields)  
  请求和响应都会用的首部

+ 请求首部字段(Request Header Fields)  
  补充了请求的附加内容，客户端信息，相应内容相关优先级
+ 响应首部字段(Response Header Fields)  
  补充了响应的附加内容，也会要求客户端附加额外的内容信息
+ 实体首部字段(Entity Header Fields)  
  补充了资源内容更新时间等与实体有关的信息

<div class="title4">5.End to End 首部和 Hop by Hop 首部</div>

+ 端到端首部(End-to-end Header)：  
  此类别的首部会转发到最终接受目标：要求必须被转发
+ 逐跳首部(Hop-by-hop Header):
  此类别首部只对单次转发有效，通过缓存或代理将不再转发。另，1.1/HTTP 之后，使用逐跳首部必须提供 Connection 首部字段。

<div id ="http-6-2" class="title3">6.2 http首部字段详细</div>

[首部字段详解](https://blog.csdn.net/xsvklftj/article/details/110008840)

-----
-----
<div id ="http-7" class="title2">七、https</div>

-----
<div  class="title3">http不足</div>
<div  class="title3">认证</div>
<div  class="title3">SSL握手</div>
<div  class="title3">完整性保护</div>


> http + 加密 + 认证 + 完整性保护 = https

（有待更新）

-----
-----
<div id ="http-8" class="title2">八、用户身份认证</div>

-----
某些Web页面只想让部分人或者仅仅让本人浏览。为达到这个目标我们需要认证功能。

<div class="title4">何为认证</div>

为了确认使用者是否有登陆者权限，需要核对“登陆者本人才知道的信息”或是“登陆者本人才会有的信息”

这些信息通常为：
+ 密码：只有本人才知道的字符串信息

+ 动态令牌：仅限本人持有设备内显示的一次性密码
+ 数字证书：仅限本人（终端）持有的信息
+ 生物认证：指纹和虹膜等本人的生理信息
+ IC卡：仅限本人持有的信息

<div id ="http-8-1" class="title3">8.1 BASIC 认证</div>

> BASIC 认证(基本认证)是1.0就定义的认证方式。是Web服务器和通信客户端之间进行的认证方式。

认证步骤：
~~~
     发送请求------------------------------>
     <--------------------返回401告知需要认证
     用户ID和密码以Base64方式编码发送-------->
     <----------认证成功返回200，不成功返回401
~~~

>显而易见，传递的信息明文解码后就是用户名和密码，因此在非加密的HTTP线路上传输并不安全。另外，一般浏览器也无法进行认证注销操作。
因为其不够灵活、安全，因此并不常用。

<div id ="http-8-2" class="title3">8.2 DIGEST 认证</div>

>DIGEST认证 ：1.1启用，为弥补BASIC缺陷。采用质询/响应的方式(challenge/response),但不会像BASIC一样明文发送。

质询响应方式：
~~~
认证要求---------------->
<------------------质询码
响应码(根据质询码计算)--->
~~~
因为响应码是根据质询码计算得到，因此比起BASIC认证，密码泄露概率就低了。

DIGEST认证概要也类似，区别在于返回的值为响应码和响应概要。（最后200表示认证成功，401表示认证失败）


>DIGEST认证提高了BASIC认证的安全等级，但是仍比不上HTTP客户端认证。  
其提供了防止密码被窃听的机制，但并未防止用户伪装。  
DIGEST和BASIC一样，使用不灵活，也达不到使用者的安全需求，因此使用范围也有限。

<div id ="http-8-3" class="title3">8.3 SSL 客户端认证</div>

<a href = "#http-7">具体</a>

一般情况下，SSL客户端不会仅靠证书完成认证，一般会和基于表单认证组合成双因素认证。前者确认是客户端计算机，后者确认是用户本人。
通过双因素认证后，就可确认是本人使用匹配的计算机访问服务器。

<div id ="http-8-4" class="title3">8.4 基于表单认证</div>

表单认证并未在HTTP协议中定义。但因为其便利性和安全性，大部分认证均为表单认证。

<div  class="title4">cookie 保存session</div>

http没有提供状态管理的功能，因此通过cookie保存session ID来保存会话信息来完成这个功能。
<div  class="title4">盐（salt）</div>

用给密码加盐(salt)的方式增加额外信息，再用散列函数计算出散列值后保存。

>盐为随机生成的字符串（足够长），和密码前后拼接。因为盐是不同的，因此散列值也是不同的，可以极大的减少密码的特征性，防止被破译。

-----
-----
<div id ="http-9" class="title2">九、基于http的功能追加协议</div>

-----
Http虽简单快捷，但随着时代的发展仍疲态尽显。因此出现了一些补充协议试图解决WEB的瓶颈。HTTP2.0也在指定规划中。  
HTTP/2.0 七项核心技术：
|技术|协议|
|---|---|
|压缩|SPDY,Friendly|  
|多路复用 |SPDY|
|TLS义务化|Speed+Mobility|
|协商|Speed+Mobility,Friendly|
|客户端拉曳(Client Pull)/服务器推送(Servel Push)|Speed+Mobility|
|流量控制|SPDY|
|WebSocket|Speed+Mobility|

-----
-----
<div id ="http-10" class="title2">十、构建web内容的技术</div>

-----

略。（html,css,servlet(java)，json，RSS/Atom等的简单介绍）

-----
-----
<div id ="http-11" class="title2">十一、web的攻击技术</div>

-----

<div id ="http-11-1" class="title3">11.1 因输出值转义不完全引发的安全漏洞</div>

<div class="title4">跨站脚本攻击</div>
<div class="title4">SQL注入攻击</div>
<div class="title4">OS命令注入攻击</div>
<div class="title4">HTTP首部注入攻击</div>
<div class="title4">邮件首部注入攻击</div>
<div class="title4">目录遍历攻击</div>
<div class="title4">远程文件包含漏洞</div>

<div id ="http-11-2" class="title3">11.2 因设置或设计上的缺陷引发的安全漏洞</div>

<div class="title4">强制浏览</div>
<div class="title4">不正确的错误消息处理</div>
<div class="title4">开放重定向</div>

<div id ="http-11-3" class="title3">11.3 因会话管理疏忽引发的安全漏洞</div>

<div class="title4">会话劫持</div>
<div class="title4">会话固定攻击</div>
<div class="title4">跨站点请求伪造</div>

<div id ="http-11-4" class="title3">11.4 其它安全漏洞</div>

<div class="title4">密码破解</div>
<div class="title4">点击劫持</div>
<div class="title4">DoS攻击</div>
<div class="title4">后面程序</div>

-----
-----
<div id ="http-ex" class="title2">EX、额外</div>

-----
<div class="title3">参考目录</div>

《图解HTTP》 [日]上野宣  2014.5

-----