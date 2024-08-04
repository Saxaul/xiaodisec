![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114368892-41a46eb6-eb4a-4d89-a110-0dbf74b8897a.png)

```plain
#知识点:
1、PHP-全局变量$_SERVER
2、MYSQL-插入语法INSERT
3、输入输出-XSS&反射&存储
4、安全问题-XSS跨站&CSRF等
```

小迪博客-输入输出&留言板&访问获取

```plain
输入输出演示：
输入代码：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363024-eff5fa86-c6e6-4c8a-995e-b73ce6e43d05.png)

输出代码，经过数据库处理：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363029-6e1cb40e-8205-4393-99bd-2d0804305da7.png)

```plain
1. 留言内容在数据库里面
2. 加载前面的前端内容
3. 可以提交留言
4. 提交的留言之后会在加载
留言板演示：
前端演示，发送数据
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363029-d0f974de-0bed-45b1-b0aa-4a81bf3ad627.png)

后台输出数据，会在页面中显示：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363083-0db2c783-bf95-4320-a991-e058b8de3114.png)

```plain
访问来源：
IP地址，浏览器信息，设备信息等等。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363295-78ba09fb-4219-481f-9272-b2706318aee3.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363507-3f99b314-469d-448d-8ce8-4029d480d440.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363665-f8d023b6-be51-413e-ac98-45e17658ac3c.png)

可以获取的参数：参考：https://www.cnblogs.com/xishaonian/p/6160893.html

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363798-bc89fd05-e1b9-4c70-bd34-d6cd505d903d.png)

https://ip.chinaz.com/ 获取IP地址，浏览器信息，Windows信息等等，可以通过抓包修改。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363818-65b51778-52a4-43ca-a879-4e451d15e1bf.png)

抓包修改后：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114363993-99be9cc7-b145-4e8e-9b6d-dff3e404b3af.png)

墨者靶场-IP地址伪造来源&来源页伪造

IP地址获取：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114364137-1aee1a09-58ef-4dce-bb1e-6b60bffedc9f.png)

```plain
来源页获取：
https://blog.csdn.net/bnrmaster/article/details/52952095
Csrf:
1.
已知工具这知道的某个管理员的习惯，喜欢看色色
攻击者在色色上面植入了一个js代码能获取管理员管理的网站信息

管理员去访问一个色色会触发代码
由于管理员浏览器可以直接登录网站，管理权限。
管理的网站：检测来源，过滤了攻击
2.
www.alipay.com/pay?name=xiaodi&money=1000&account=1307023024@qq.com
我在我博客注入访问URL的代码：访问这个付款数据包。而这个时候你打开了我的博客并且你在支付宝处于登录状态。这个时候就会触发这个数据包，执行支付。支付宝检测到这个付款URL是从小迪博客过来的，GG。
```





来自: [安全的小菜鸡 - 第14天：PHP开发-个人博客项目&输入输出类&留言板&访问IP&UA头&来源](http://www.mumuxi8.com/?id=17)