---
title: 服务器简单操作MongoDB
date: 2020-10-13 21:14:34
categories:
- linux
tags:
- dictionary
- MongoDB
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

首先登陆自己的系统，不提。

-----
<div class="title2">一、安装</div>

-----
<div class="title3">1.1 安装Docker</div>

用Docker安装MongDB，比较自动，方便。
~~~
sudo yum -y update
sudo yum -y install epel-release
sudo yum -y install docker-io
~~~
分别更新系统所有包，安装扩展包，安装docker.注意不能写install docker.  
使用root账号可去除sudo.

<div class="title3">1.1 安装MongoDB</div>

<div class="title4">启动Docker</div>

~~~
sudo systemctl start docker
sudo docker version
~~~

<div class="title4">安装MongoDB并启动</div>

~~~
sudo docker pull mongo:latest
sudo docker images

sudo docker run -itd --name mongo -p 27017:27017 mongo --auth
~~~
前两行安装，后一行启动
（别的我都懂，但是真的装的好慢啊）

~~~
sudo docker ps
~~~
>检查是否启动成功

MongoDB默认端口号为27017

-----
-----
<div class="title2">二、创建账户</div>

-----
<div class="title3">2.1 登录</div>

<div class="title4">登录数据库</div>

~~~
sudo docker exec -it mongo mongo admin
~~~
该命令为登入MongDB软件控制台，出现 `>` 后，后面可以输入数据库操作命令了

<div class="title4">创建管理员账户</div>

~~~
db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'root', db: 'admin'}]});
db.auth('admin', '123456')
~~~
创建完admin账户后，才可以继续创建数据库步骤
>注意，管理员账户不能读写数据库。管理员是数据库软件MongoDB的管理员，只有软件的实例————数据库的用户可以读写数据库

-----
-----
<div class="title2">三、创建数据库实例</div>

-----
均需在MongoDB登录状态下执行

<div class="title3">3.1 切换数据库</div>

`use`是创建并切换到指定数据库的意思，若已存在就直接切换
~~~MongoDB
use practice
~~~

<div class="title3">3.2 创建读写用户</div>

~~~MongoDB
db.createUser({ user:'xxxx',pwd:'xxxxxx',roles:[{ role:'root', db: 'admin'},{ role:'dbAdmin', db: 'practice'}]});
~~~

<div class="title4">验证</div>

~~~MongoDB
db.auth('xxxx', 'xxxxxx')
~~~
返回1代表成功了。

-----
-----
<div class="title2">四、退出登录</div>
-----
退出MongoDB
~~~
exit
~~~

-----
-----
<div class="title2">附录</div>
-----
需记住MongoDB的端口号，数据库名，数据库的用户名和密码。

注意你的服务器的安全设置，需开放27017端口。

-----