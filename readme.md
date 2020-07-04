## 有关命令行的那些事
一些命令
+ 查看SSH内容（可以检查是否生成了本地SSH）
~~~cmd
cat ~/.ssh/id_rsa.pub
~~~
+ 生成本地SSH
~~~cmd
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
~~~
![](https://qgt-style.oss-cn-hangzhou.aliyuncs.com/newcoursep4/g1/g1-2-2/tenor.gif)
