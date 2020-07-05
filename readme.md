<style>
.title1{
    font-size:36px;
    color:#ffd600;
}
.title2{
    font-size:29px;
    color:#ffea00;
}
.title3{
    font-size:22px;
    color:#ffff00;
}
.title4{
    font-size:15px;
    color:#ffff8d;
}
</style>

<div class="title1">有关命令行的那些事</div>
------
<div class="title3">常见的Linux命令</div>

>时刻注意自己的路径
+ 查看当前目录
~~~cmd
ls
~~~
>若要包括隐藏文件，则使用：`ls -a`
+ 当前目录创建文件
~~~cmd
touch 文件
~~~
+ 当前目录创建文件夹
~~~cmd
mkdir 文件夹
~~~
+ 进入某文件夹
~~~cmd
cd 文件目录
~~~
+ 删除当前目录制定文件(<span style="color:red">慎用</span>)
~~~cmd
rm -rf 文件
~~~
>仓库地址在仓库中获取（ssh）
---
<div class="title3">有关git</div>
<div class="title4">全局配置</div>

+ 告诉git你是谁
~~~cmd
git config --global user.name "your_name"
~~~
+ 告诉git你的邮箱
~~~cmd
git config --global user.email "your_email@example.com"
~~~
<div class="title4">clone</div>

+ git clone(将github上的仓库下载到本地)
~~~cmd
git clone 仓库地址
~~~
>仓库地址在仓库中获取（ssh）
<div class="title4">Git提交三部曲</div>

+ git add
~~~cmd
git add -A
~~~
>`git add .`命令效果与之前相同，此处`-A`指all.
+ git commit
~~~cmd
git commit -m "本次提交的修改备注"
~~~
>使用`git commit -am "本次修改备注"`来合并之前的步骤
+ git push
~~~cmd
git push oringin master
~~~
>第一次提交到本分支，`origin`是远程仓库默认名称，`master`是分支名称
>第2-n次提交到相同分支时，可简化为`git push`;同时，若仓库和本地不一致，应先执行`git pull`
<div class="title3">有关ssh</div>

+ 查看SSH内容（可以检查是否生成了本地SSH）
~~~cmd
cat ~/.ssh/id_rsa.pub
~~~
+ 生成本地SSH
~~~cmd
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
~~~
-----
![](https://qgt-style.oss-cn-hangzhou.aliyuncs.com/newcoursep4/g1/g1-2-2/tenor.gif)
