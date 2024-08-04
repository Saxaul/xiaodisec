### #知识点

1、Javaweb常见安全及代码逻辑

2、目录遍历、身份验证、逻辑、JWT

3、访问控制、安全组件、越权、三方组件

## Javaweb常见安全及代码逻辑

Javaweb框架学习-使用WebGoat8靶场搭建

地址：[GitHub - WebGoat/WebGoat: WebGoat is a deliberately insecure application](https://github.com/WebGoat/WebGoat)



JWT实际上就是一个字符串，它由三部分组成，头部（加密方式）、载荷、签名。前两部分经过base64编码，后一部分通过前两部分base64编码后再加密而成

补充学习：[JWT原理及常见攻击方式 - yokan - 博客园](https://www.cnblogs.com/yokan/p/14468030.html)



JWT攻击思路

1、空加密算法

2、爆破密钥

3、KID利用，和其他漏洞组合攻击





\#访问控制
隐藏属性：前端页面的自慰限制显示
水平越权：同一级别用户权限的查看
**安全问题-访问控制&安全组件第三方组件**
\#安全组件
联想到刚爆出的Log4j2

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744697-4a201a27-b6f2-48e5-a5a0-310d6cda251a.png)

```plain
#知识点：
1、JavaWeb常见安全及代码逻辑
通过URL信息来对应源码文件。没有代码的情况下很难渗透。
2、目录遍历&身份验证&逻辑&JWT
3、访问控制&安全组件&越权&三方组件
```

- JavaWeb-WebGoat8靶场搭建使用

```plain
• https://blog.csdn.net/Petrichoryi/article/details/105930751
• 压缩到jar包里面。
• 如何启动：安装jdk，运行java.exe -jar webgoat --server.port= 9091
• 
• java -jar webgoat-server-8.1.0.jar --server.port=8091
访问ip+WebGoat/login.html，注册账号登录就可以了。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744501-5d27f404-5c73-4db3-89f5-07f62c3e1baf.png)

- 安全问题-目录遍历&身份认证-JWT攻击

```plain
目录遍历：
解决问题：当前目录不能执行，上传至别的目录进行执行脚本
上传上去的地址：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114743960-bb2593cb-2cd0-4bb2-bb4e-8067850918bb.png)

目标地址：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114743952-8aa1d684-3928-48a0-9ac4-4c04f6779c2d.png)

抓取上传数据包：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744017-3ad724a3-cefb-46f0-b92e-d427be427bc6.png)

找到相对应触发的代码段：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744684-973400a4-5838-4755-838b-8f210b153cfa.png)

```plain
关键代码：
publicAttackResultuploadFileHandler(@RequestParam("uploadedFile")MultipartFilefile,@RequestParam(value="fullName",required=false)StringfullName){
returnsuper.execute(file,fullName);
}

接受uploadedfile
对应抓包：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744659-e71578fd-a273-46c1-9005-c169e4fde1fe.png)

把test修改为1后，发现返回数据包：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744604-16e87b89-a1e4-448b-9607-0f117c28198a.png)

把fullname修改为：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744807-de46d59e-0042-43ce-b578-a8b5cd661fe8.png)

```plain
成功过关。
第三关，用同样的方式没有用了，打开相对应的代码，发现关键的过滤：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744853-36e90518-ce5c-425d-b0b4-29923ef15658.png)

```plain
但是这个过滤知识一次性的，并不是很严格。
双写绕过：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114744943-9d68fd7b-d40f-417b-a89d-81c54183b027.png)

解决问题：当前上传的文件所在的文件夹不解析。

```plain
身份认证：
认证问题答案，很多选项：
你叫什么名字等等.....只有两个问题

接受键名和键值
s0=xiaodi&s1=湖北 正确
s3=null&s4=null发送数据
s3 s4为空，相当于NULL，那么就能进行绕过。
安全验证：
固定接受的数据：s0 s1判断你的数据 正常
不固定：s0 s1 正常
不固定：s2 s3不在数据库或者变量内，攻击者就能测试。
URL触发连接，访问地址：auth-bypass/verify-account
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745135-7059412f-aa7a-4417-a2a8-8fe0f1d1a74c.png)

找到对应的代码段

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745195-5f6f6526-11bc-4e79-94b8-88b74383c672.png)

```plain
JWT：
https://www.cnblogs.com/vege/p/14468030.html
三部分构成，以.分隔，出现在cookie上。
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
对比cookie
解密平台：https://jwt.io/

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745307-fecfee98-34e2-42f8-b9f4-e2f7998a4788.png)

切换账号，抓包发现：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745237-100b2275-7d48-4d11-8a04-fba77f671d51.png)

```plain
cookie：eyJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE2NDMxOTk5MzYsImFkbWluIjoiZmFsc2UiLCJ1c2VyIjoiVG9tIn0.C_2nlYK5_bhv5RHnRbPW7sxIkZsaPN-AYX3xBwo3n5PeIzdK7G1-sjJjo8qIaH_JHZVaR7amGQTdNiEjYAkAMg

解密后：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745532-58dd0640-7e8b-469a-9175-3b60e9a67660.png)

```plain
攻击：1.空加密算法 2.爆破 3.KID
1.空加密算法：
生成字符串：算法模式+秘钥（缺一不可）
忽略秘钥生成，需要服务器支持不要秘钥签名（空模式加密）
抓到数据包
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745525-554b6950-4914-40ae-9ddd-bd31e7cd5d81.png)

```plain
找到相对应的代码段发现可以空加密然后用python脚本去写空加密jwt
eyJhbGciOiJub25lIn0.eyJpYXQiOjE1NzM0NzAwMjUsImFkbWluIjoidHJ1ZSIsInVzZXIiOiJKZXJyeSJ9.


2.爆破：
```

- 安全问题-访问控制&安全组件-第三方组件

```plain
-联想到刚爆出的Log4j2
<sorted-set>
 <string>foo</string>
 <dynamic-proxy>
 <interface>java.lang.Comparable</interface>
 <handler class="java.beans.EventHandler">
 <target class="java.lang.ProcessBuilder">
 <command>
 <string>calc.exe</string>
 </command>
 </target>
 <action>start</action>
 </handler>
 </dynamic-proxy>
</sorted-set>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114745641-e3ef433e-e2d5-4ebe-a81e-cdada33aa3ab.png)