## 信息打点-系统篇&端口扫描&CDN服务&负载均衡&WAF防火墙

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345623849-a9df884f-010a-4635-b741-f59bfc328899.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345626779-49159fd3-38e7-4e6d-9356-664ea5dcb6e7.png)

## 一、知识点

1、获取网络信息-服务器厂商：

阿里云，腾讯云，机房内部等。

网络架构：

内外网环境。

2、获取服务信息-应用协议-内网资产：

FTP，SSH，redis 协议 等。

3、获取阻碍信息-CDN服务-WAF防火墙，负载均衡，防火墙阻碍

```plain
#相关利用项目：
Masscan：https://github.com/robertdavidgraham/masscan
Wafw00f：https://github.com/EnableSecurity/wafw00f
Kali上自带Nmap，Masscan，lbd等项目，超级ping：ping.chinaz.com

超级ping：CDN服务识别
Masscan：端口扫描，应用协议
Wafw00f：Web应用防护防火墙识别
Nmap：端口扫描，应用协议，防火墙识别
lbd：负载均衡，广域网负载均衡，应用层负载均衡

#端口协议安全：	https://www.se7ensec.cn/2018/11/28/%E7%AB%AF%E5%8F%A3%E6%B8%97%E9%80%8F%E6%80%BB%E7%BB%93/
```

## 二、网络信息获取-服务厂商&网络架构

域名通过IP地址查询获得厂商信息

[iP地址查询–手机号码查询归属地 | 邮政编码查询 | iP地址归属地查询 | 身份证号码验证在线查询网 (ip138.com)](https://www.ip138.com/)等网站

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624041-36459f4e-b595-4513-a3cf-64ab78a5dfc2.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345623728-3382e9e1-952b-4c6b-aec8-40df62235d43.png)

```plain
准备Windows2012 部署宝塔 搭建一个网站www.123.com
登录到路由器做端口映射，把171.40.78.83的80端口进行映射到192.168.1.13端口上
www.123.com 171.40.78.83
web 服务器 192.168.1.13
这时候扫描171.40.78.83 这时候就会误报或者扫不到，不会扫到真实主机。这个时候就扫不到，没办法解决的。
只在部分企业，学校内部出现。
```

## 三、服务信息获取-协议应用&内网资产

### 1、端口

协议应用：扫描端口：常见默认端口及对应测试方法

nmap扫描：

https://wiki.wgpsec.org/knowledge/tools/nmap.html

常用参数

```plain
nmap -T4 -A -v -Pn IP	
#最常用的一种扫描

-T4		#设置时序，越高扫描越快
-A		#启用操作系统检测，版本检测，脚本扫描和跟踪路由
-v		#增加详细级别（使用-vv或更高级别以获得更好的效果）
-Pn		#无ping扫描
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345623713-3249bd86-9bae-414c-90c6-d4f0ab579dd4.png)

masscan扫描：![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345623949-0bb395be-8e3a-450b-a1e7-25f0da1f82e3.png)

[端口渗透总结 - Se7en’s Blog|专注渗透测试。 (se7ensec.cn)](https://www.se7ensec.cn/2018/11/28/端口渗透总结/)

[重要端口及服务速查-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134692278?spm=1001.2014.3001.5501)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624316-08dee1c9-cd6d-4da7-a16a-387e8be8a3a8.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624326-38b72b56-dced-41ca-9572-4950ea90a1f0.png)

### 2、旁注

同一个IP，服务器下，有多个不同的网站

在线查询：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624839-b3df2894-3625-4a71-8205-f38b868f38c4.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624672-56272fb2-9b74-4079-be9b-cf73e975cf14.png)

### 3、c段

同网段，直接通过nmap或者masscan进行扫描。

```plain
扫网段：

目标：171.40.78.83
网段：171.40.78.1-255 同一个网段
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624612-6ac4dc9f-e655-4833-8874-f51c9dcd39f3.png)

扫描出的ip信息，可以通过ip反查获取域名：

#### IP反查域名

- IP138 https://site.ip138.com/
- 微步在线 https://x.threatbook.cn/
- VirusTotal [https://www.virustotal.com/](https://www.virustotal.com/gui/home/upload)

旁注结合C段进行探测。扫描到C段后，探测旁注信息。

## 四、阻碍信息获取-CDN&WAF&负载&防火墙

### 1、CDN：

```plain
在网站其他地区创造一个结点，解决访问速度问题。
```

假如你在福州访问就会在福州给你创造一个节点，就近选择节点。

对于获取真实ip会造成阻碍。

判断：多地同时ping，看看IP是否一致

超级ping https://ping.chinaz.com/

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624693-7a419a62-db5f-4cad-bbb0-3279c0ff2e5a.png)

### 2、WAF识别

waf是web防火墙

```plain
看图识别：https://www.cnblogs.com/AdairHpn/p/13985760.html
```

通过拦截页面分析，waf厂商识别很重要，大型网站一般很难绕过。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625097-b7c421f6-bdcd-40cd-b0b4-a828d53b7fb3.png)

```plain
工具识别：wafw00f检测waf工具使用
https://blog.csdn.net/weixin_45851870/article/details/119728258

下载地址：https://github.com/EnableSecurity/wafw00f
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345624994-b55f344d-c778-4dc4-b134-06d0e5c84c73.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625157-6fa4fbb1-f49e-4c7d-912e-a85e9d079f94.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625124-3e802df1-b43f-498c-8b2c-899d137a892e.png)

### 3、负载均衡识别

识别工具：lbd

kali自带

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625487-a2fd495a-eacb-40bf-bbf9-3289f5cd47e7.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625335-50d3a3a0-06a2-4853-8c94-7f877aa8e425.png)

### 4、防火墙

```plain
常见的系统自带 
及硬件防火墙
一般与内网渗透有关联
外网：访问不到，那就基本上没有作用测试。
入站规则，出站规则。一般看入站规则
对端口扫描有一定的阻碍
```

[Kali渗透测试之服务扫描3——防火墙识别、负载均衡识别、WAF识别_the server returns a different response code when -CSDN博客](https://blog.csdn.net/weixin_43625577/article/details/89648132)

- nmap有系列防火墙过滤检测功能

可根据下面四种方法来判断防火墙的过滤规则：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345625621-de61b0ad-6308-4c4f-83ce-f0f617dff77d.png)



来自: [第八天：信息打点-系统&端口&CDN&负载均衡&防火墙_se7ensec's blog-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134699996?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113734856-1996ee30-169c-44b6-b0dd-ead0f7f0ab66.png)

```plain
#知识点：
1.获取网络信息-服务厂商&网络架构
阿里云，华为云，腾讯云。或者学校内部（自己的机房），大型公司（自己的机房）。
2、获取服务信息-应用协议&内网资产
3、获取阻碍信息-CDN&WAF&负载&防火墙




#详细点:
CDN服务，WAF防火墙，负载均衡，防火墙阻碍？
#相关利用项目：
Masscan：https://github.com/robertdavidgraham/masscan
Wafw00f：https://github.com/EnableSecurity/wafw00f
Kali上自带Nmap，Masscan，lbd等项目，超级ping：ping.chinaz.com

超级ping：CDN服务识别
Masscan：端口扫描，应用协议
Wafw00f：Web应用防护防火墙识别
Nmap：端口扫描，应用协议，防火墙识别
lbd：负载均衡，广域网负载均衡，应用层负载均衡

#端口协议安全：	https://www.se7ensec.cn/2018/11/28/%E7%AB%AF%E5%8F%A3%E6%B8%97%E9%80%8F%E6%80%BB%E7%BB%93/
```

- 网络信息获取-服务厂商&网络架构

```plain
服务厂商：
服务器所在地方可能是腾讯云或者阿里云或者本地搭建
可以知道防护难度大不大，防护情况是怎么样的，换一种思路进行渗透
可以基于IP地址域名收集到 https://www.ip138.com/
查询，找相关信息，电话号码，注册域名商等：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730056-49d36fb7-5f97-41a3-ad5c-8f9a50e7a7c0.png)

```plain
网络架构：
如果是本地搭建，一般会在内网搭建，有映射关系
如果是阿里云等，一般是外网服务器搭建

一般在企业，学校部署：
外网：171.40.78.83  内网 192.168.1.100 web
外网出口（路由器，交换机）做一个映射 反向代理
主动把192.168.1.100web流量给到171.40.78.83
作为安全测试：
扫描探针信息：171.40.78.83，这样就扫不到想对应的端口

准备Windows2012 部署宝塔 搭建一个网站www.123.com
登录到路由器做端口映射，把171.40.78.83的80端口进行映射到192.168.1.13端口上
www.123.com 171.40.78.83
web 服务器 192.168.1.13
这时候扫描171.40.78.83 这时候就会误报或者扫不到，不会扫到真实主机。这个时候就扫不到，没办法解决的。
只在部分企业，学校内部出现。
```

- 服务信息获取-协议应用&内网资产

```plain
协议应用： 扫描端口，常见默认端口https://www.se7ensec.cn/2018/11/28/%E7%AB%AF%E5%8F%A3%E6%B8%97%E9%80%8F%E6%80%BB%E7%BB%93/
本机应用协议：ssh ftp Redis 等服务
nmap 用法：https://blog.csdn.net/m1585761297/article/details/80015726
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730069-52d963fa-ba42-4602-95da-be1e86c504e5.png)

masscan 用法： https://blog.csdn.net/weixin_46789316/article/details/110383622

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730049-61eb1f09-2f80-4104-8ec0-86cbd6988553.png)

```plain
内网资产
扫网段：

目标：171.40.78.83
网段：171.40.78.1-255 同一个网段
如果获取到171.40.78.82 这样就可以进行内网之间的渗透测试
旁注：同服务器下面的不同web应用查询技术
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730099-b175b8c3-056a-4354-9e17-2abff20b4cee.png)

c段：同网段下面的不同服务器IP下的web应用查询技术

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730348-e292640b-4f82-4e65-be00-6be8c1bf76e0.png)

```plain
旁注：171.40.78.83
www.123.com 目标 没有安全测试头绪，转到www.456.com
www.456.com 
www.789.com

171.40.78.83
www.123.com
171.40.78.1-255所有网站的协议服务 masscan可以进行查询

得到IP后，可以进行IP反查技术进行信息收集。
```

- 阻碍信息获取-CDN&WAF&负载&防火墙

```plain
CDN：
在网站其他地区创造一个结点，解决访问速度问题。
www.xiaodi8.com 47.75.212.155（真是IP）
开通了CDN服务 安全测试相当于对于节点进行测试，只是一个虚构的东西
北京：北京IP
南京：南京IP。。。。
判断：超级ping https://ping.chinaz.com/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730365-242025cc-b606-4598-b408-7cb7a57ae451.png)

```plain
WAF：
看图识别：https://www.cnblogs.com/AdairHpn/p/13985760.html
waf厂商识别很重要，大型网站一般很难绕过。
工具：wafw00f-master https://blog.csdn.net/weixin_45851870/article/details/119728258
搜素：61.144.49.110:8005 www.tn2000.com 
安装：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730963-8b65465d-99a5-402b-a252-9411fe275163.png)

使用G:\xiaoditools2\008-工具项目&URL资源等\wafw00f-master\build\lib\wafw00f：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730712-2fb6e61f-7913-43bc-b139-642ce866fa7b.png)

识别：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113730836-58933cde-3365-4f22-8f80-ae656dbf056f.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113731316-d7dcb619-5b36-406d-8624-8faf73c4bc5a.png)

```plain
负载：kali自带ldb
ldb:https://blog.csdn.net/qq_30247635/article/details/86428154
相当于cdn
识别xiaodi8.com
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113731191-adb9749e-2011-4e72-992b-271b450a77ab.png)

识别baidu.com

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113731147-1a949387-4386-4178-ab2a-0f38e564b1d8.png)

```plain
防火墙：
系统自带 硬件防火墙
一般与内网渗透有关联
外网：访问不到，那就基本上没有作用测试。
入站规则，，出站规则。一般看入站规则
对端口扫描有一定的阻碍
识别：https://blog.csdn.net/fageweiketang/article/details/84642049
```





来自: [安全的小菜鸡 - 第8天：信息打点-系统篇&端口扫描&CDN服务&负载均衡&WAF防火墙](http://www.mumuxi8.com/?id=11)