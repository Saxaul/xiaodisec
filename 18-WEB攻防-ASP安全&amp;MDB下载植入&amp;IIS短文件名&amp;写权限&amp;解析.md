![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680091-967cf66e-9ac2-45b4-830a-5212c2919edb.png)

```plain
#知识点：
1、ASP环境搭建组合
2、ASP-数据库下载&植入
3、IIS-短文件&解析&写权限

WEB安全攻防：
1、Web源码
2、开发语言
3、中间件平台
4、数据库类型
5、第三方插件或软件
```

- ASP-数据库-MDB默认下载

```plain
Windows +iis + asp+ access(sqlserver)
Access 数据库一般后缀名（asp asa mdb(下载)）
mdb文件在他网站目录下（扫描得到，或者默认数据库配置）
默认：
C:\Inetpub\wwwroot\PowerEasy\Database
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114679966-a69dc468-3c2c-465c-9b38-07ef8bdd9f85.png)

```plain
思路：如果我们知道这个数据库的地址，尝试可以下载获取数据库文件，获取当前管理员账号密码信息
访问：http://192.168.199.136:83/Database/powerseasy2006.mdb
就可以进行直接下载：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114679956-db0656c7-550a-4506-92d3-8abf0b63e124.png)

如果有解析配置文件，那么就进行解析，如果没有，则默认下载：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114679967-d97b7058-e4cf-4c98-807b-b2d78e650c1d.png)

- ASP-数据库-ASP后门植入连接

数据库目录文件：C:\Inetpub\wwwroot\aspcms\data，，数据库文件是以asp解析。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114679958-6d7625c7-3625-40e2-a7a3-e8b560ee2639.png)

直接访问：http://192.168.199.136:82/data/data.asp

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680860-29ec8b1e-16d6-4546-8011-0073aa6ad30d.png)

如果留言等会有记录，那么写入后门代码，后门代码记录在数据库中，用菜刀连接执行。后门代码：┼攠數畣整爠煥敵瑳∨≡┩愾 ，连接密码为a

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680546-53ee0456-2021-4904-8cfe-6e3fb47ca38f.png)

- ASP-中间件-IIS短文件名探针-安全漏洞

```plain
用于不知道地址怎么探针，参考：https://www.freebuf.com/articles/web/172561.html
能够扫描到目录结构，探针数据库或者后台文件。
使用到iis_shortname_Scan.py：执行命令python2 iis_shortname_Scan.py http://192.168.199.136:82/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680268-6eba6a58-04e2-4921-a6a2-bd033905c610.png)

执行后的结果：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680917-00d30931-158d-437a-b217-54ec6be6a663.png)

扫描借助的是字典，他借助的是漏洞。URL可以接目录的下一级。

- ASP-中间件-IIS文件上传解析-安全漏洞

```plain
账号密码：admin panfei806
登录后台后，访问http://192.168.199.136:81/upload.asp
上传1.asp后，能够访问。不能上传
上传1.jpg后，不能访问，能上传
上传111.asp;.jpg后，能够访问，能上传
假如有a.asp目录，里面有文件1.jpg 可以执行，能上传，但是目录不一定有。

模拟正常的数据包上传：返回地址/202211215412767127.jpg
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680802-0965a6f5-b247-46ed-9aa7-56de92c1f8e9.png)

发现文件数据包：filepath参数

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681036-c5e18f37-4b91-4dbe-b6ec-748984eeaf6e.png)

修改参数：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114680875-84bbede8-e951-4f5c-890d-89c3a5ccc5e0.png)

就可以修改了，但是错误。

- ASP-中间件-IIS配置目录读写-安全配置

如果在配置过程中：点了写入

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681138-919aa781-ee9d-474d-a7e8-4ba73da05ca8.png)

开启webdav:，就会产生写入漏洞。需要满足两个条件

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681247-de117107-bc48-4095-91d1-7ddeee7e279c.png)

如何判断这类型的漏洞，用到iisputscaner工具。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681493-a7152ed1-15b3-4863-be47-6e7165e4dabd.png)

利用，先put一个文件，一张图片里面带有后门的

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681277-cf4fcab0-037f-4730-b980-7ba01e4516ca.png)

put完后然后进行移动：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114681369-79fec126-0f50-4f3d-a751-b0779dc50200.png)



来自: [安全的小菜鸡 - 第18天：WEB攻防-ASP安全&MDB下载植入&IIS短文件名&写权限&解析](http://www.mumuxi8.com/?id=21)