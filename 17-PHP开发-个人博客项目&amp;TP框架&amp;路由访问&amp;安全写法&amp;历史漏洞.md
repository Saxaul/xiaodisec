```plain
#知识点：
1、基于TP框架入门安装搭建使用
2、基于TP框架内置安全写法评估
3、基于TP框架实例源码安全性评估
```

- 入门-简单了解-安装&调试&入口&配置

```plain
理解：TP框架架构&配置&查看等 
用phpstudy加载运行，选中目录要选public
调试开关：在application中的config.php
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114643979-5bc2492c-6057-4d99-9d1e-e89015812304.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644019-90565cf2-b5f8-462d-8146-e89a2819417f.png)

SQL语句监控要用官方的写法才能监控。

- 使用-路由访问-控制器&对象&函数&参数

```plain
理解：URL <=> 文件
理解路由地址，应该怎么在URL访问他。
访问http://127.0.0.1:8084/index.php/index/index/index对应的文件是：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114643980-1f0d2201-0c19-49ff-a4b7-c86e440cf542.png)

Index.php下面的index方法，第一个index代表着index文件夹，第二个index代表着index/controller/index文件，第三个index代表着index.php下的index方法。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114643988-653af6c8-7054-4ecd-b5f3-fc5744ddeb47.png)

如果需要访问Xiaodi.php的内容，那么就构造URL：http://127.0.0.1:8084/index.php/index/xiaodi/x

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644027-29d64225-5eeb-4568-881d-a89937325272.png)

参数传递：写法

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644236-a7364876-ce0e-4269-8abd-d57e64f5ed5f.png)

构造URL：http://127.0.0.1:8084/index.php/index/xiaodi/x?id=1

- 安全-SQL注入-不安全写法对比官方写法

常规SQL写法，不引用任何框架，有安全漏洞：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644303-3f449a66-1ad1-4002-95d2-cac464b7110e.png)

1、参数过滤

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644239-e2ec2edf-d0a2-4147-a61c-3b80038b4354.png)

```plain
$id=$_GET['x']     id值可以变动
$id=input('x')       id值可以变动
$id=input('?get.x')   id值被过滤了

2、内置过滤
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644372-244e0d32-a54e-4db5-974b-adb02e638f3c.png)

```plain
Db::table('think_user')->where('status',1)->select();
相当于select * from think_user where status=1;

内置过滤：
$id=input('?get.x') ；
Db::table('think_user')->where('status',1)->select();
```

- 安全-RCE执行-历史安全漏洞对比框架版本

```plain
1、框架版本漏洞
2、框架写法安全
3、黑盒白盒看版本
https://github.com/Mochazz/ThinkPHP-Vuln


版本：
黑盒：先判断是不是tp，一般是借助报错，或者返回数据包，URL地址等等。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644322-7a2f7927-4ad6-4464-bfde-7e7a87d9f4f7.png)

```plain
白盒：看源码
白盒版本信息：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114644601-c6e6ca98-7320-4d16-bc0d-2105211351a7.png)

还可以用kunyu搜索tp框架历史漏洞。

- 实例-CMS源码-EyouCMS&Fastadmin&YFCMF

```plain
AdminLTE后台管理系统
layui后台管理系统
thinkcmf
H-ui.admin后台管理系统
tpshop
FsatAdmin
eyoucms
LarryCMS后台管理系统
tpadmin后台管理系统
snake后台管理系统
ThinkSNS
DolphinPHP后台管理系统
WeMall商城系统
CLTPHP
齐博CMS
DSMALL
YFCMF
HisiPHP后台管理系统
Tplay后台管理系统
lyadmin后台管理系统
haoid后台管理系统
```





来自: [安全的小菜鸡 - 第17天：PHP开发-个人博客项目&TP框架&路由访问&安全写法&历史漏洞](http://www.mumuxi8.com/?id=20)