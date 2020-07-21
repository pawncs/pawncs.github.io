---
title: realm使用
date: 2020-07-20 20:59:59
categories:
- Android
tags:
- realm
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
</style>

<div class="title2">一、安装</div>

将其作为Gradle插件安装。
<div class="title4">步骤1：将类路径添加到build.gradle中。</div>

~~~gradle
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath "io.realm:realm-gradle-plugin:6.0.2"
    }
}
~~~
<div class="title4">步骤2：realm-android在应用程序级别build.gradle文件顶部附近应用插件。</div>

~~~gradle
apply plugin: 'realm-android'
~~~

<div class="title4">步骤3：在application文件里将其初始化。</div>

~~~java
Realm.init(this);
~~~
[视频](https://www.imooc.com/video/19247)

<div class="title3">Realm 模型的概念</div>

+ 一个模型就是一张表，模型中的子弹表示表中的列
+ 模型需要继承RealmObject类
+ 模型实时，自动更新

<div class="title3">Realm 事务</div>

+ 所有会使数据发生变化的操作必须在事务中进行
+ 事务分为同步事务和异步事务两种
+ 两种事务共有三种写法

<div class="title4">手动同步开启事务</div>

~~~java
        //获取realm对象
        Realm realm = Realm.getDefaultInstance();
        //开启事务
        realm.beginTransaction();
        //创建被管理的实例对象，该对象的所有变更都会被直接应用到Realm数据源中
        Model model = realm.createObject(Model.class);
        //更改数据
        model.setCount(model.getCount() + 1);
        //提交事务
        realm.commitTransaction();
//         //取消事务        
//        realm.cancelTransaction();
~~~

<div class="title4">同步事务执行块</div>

~~~java
/**
*除了写法和上一种不同外并没有本质的区别
*/
realm.executeTransaction(new Realm.Transaction() {
            @Override
            public void execute(Realm realm) {
                Model model = realm.createObject(Model.class);
                model.setCount(model.getCount() + 1);
            }
        });
~~~

<div class="title4">异步事务执行块</div>

~~~java
        Realm realm = Realm.getDefaultInstance();
        RealmAsyncTask asyncTask = realm.executeTransactionAsync(new Realm.Transaction() {
            @Override
            public void execute(Realm realm) {
                Model model = realm.createObject(Model.class);
                model.setName("异步");
            }
        }, new Realm.Transaction.OnSuccess() {
            @Override
            public void onSuccess() {
                Log.d("Realm","事务成功回调");
            }
        }, new Realm.Transaction.OnError() {
            @Override
            public void onError(Throwable error) {
                Log.e("Realm","事务失败回调");
            }
        });
    }
~~~
<div class="title3">引用计数</div>

+ Realm实例使用引用计数的方式。
> 每次`getDefaultInstance`计数加一，`close`时计数减一。所以每次打开之后都要关闭。

<div class="title3">增删改查之——增</div>

+ insert或insertOrUpdate

+ copyToRealm/createObject获取自动更新的Realm模型（把new出来的对象放到Realm里。）

>在做实验的时候发现一点，即引用了Realm后，再添加Button标签，若button没有添加事件，则工程无法启动

>密码加密`setPassword(EncrytUtils.encryptMD5ToString(password))`

>未完待续



<div class="title1">附录</div>

+ 此错误为未在表AdroidManifest.xml中注册Application
~~~
java.lang.IllegalStateException: Call `Realm.init(Context)` before calling this method.
~~~