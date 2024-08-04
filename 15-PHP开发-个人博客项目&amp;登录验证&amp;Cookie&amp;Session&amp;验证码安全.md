![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114541924-22462bbb-dd8c-4cbd-975d-e47b61efc6b3.png)

```plain
#知识点：
1、后台验证-登录用户逻辑安全
2、后台验证-COOKIE&SESSION
3、后台验证-验证码&万能密码等

万能密码：是根据SQL语句来进行测试的。
账号：'or 1=1#，密码随便输入
```

- 小迪博客-后台登录&COOKIE&SESSION

```plain
1.发送登录请求 账号密码
2.接受账号密码
3.判断账号密码的准确性

正确 成功登录->跳转成功页面
错误 失败登录->重新登录

后台管理系统有多个文件页面，为了方便验证，一般会选用cookie或session进行验证

cookie：身份验证，存储在客户端浏览器内
cookie安全：cookie修改 伪造 盗取
session：身份验证，存储在服务端服务器内
session：session盗取
Cookie
前端提交：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114536015-2ad9835b-cb30-4627-b53f-0b29cee2c4b8.png)

cookie产生验证：setcookie，登录验证。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114536085-11cad70c-031f-4c7f-a0e9-4492c67a9a2b.png)

判断是否登录：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114536076-f922d039-aa73-4e3c-aa60-929d8670614c.png)

```plain
在登录后台后，查看元素，cookie值：user=admin

session
函数：session_start();
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114536262-7a6a3e90-1bc0-4628-b198-53eb68da3114.png)

在cookie中，多了一个PHPSESSID

- 本地靶场-某CMS后台登录验证COOKIE脆弱

```plain
只要检测不为空就可以登录，抓包进行修改就可以通过。
127.0.0.1:8087/admin/?index，访问需要登录，那么登录又是怎么验证的呢？进行抓包：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114536858-da87cad1-709c-4886-8bae-5dbf7d396aa4.png)

通读代码，了解cookie判断，修改数据包：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114537133-d0c2d737-acd7-4be8-b5f4-797ecfa772df.png)

放出数据包，直接通过：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114537855-dafcfe01-91b4-4338-b990-2b97d6b2aa31.png)

产生cookie：代码

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114537011-c68eb212-2d37-46fc-a686-5c4ac6ee5d21.png)



来自: [安全的小菜鸡 - 第15天：PHP开发-个人博客项目&登录验证&Cookie&Session&验证码安全](http://www.mumuxi8.com/?id=18)