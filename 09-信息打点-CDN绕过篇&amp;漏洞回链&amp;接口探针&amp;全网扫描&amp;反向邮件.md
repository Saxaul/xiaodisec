## 信息打点-CDN绕过篇

cdn绕过文章：https://www.cnblogs.com/qiudabai/p/9763739.html

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647641-d7fd5bb2-08e2-4980-a123-12686b7c110b.png)

## 一、CDN-知识点

### 1、常见访问过程

```plain
1、没有CDN情况下传统访问：用户访问域名-解析服务器IP–>访问目标主机
2.普通CDN：用户访问域名–>CDN节点–>真实服务器IP–>访问目标主机
3.带WAF的CDN：用户访问域名–>CDN节点（WAF）–>真实服务器IP–>访问目标主机
```

### 2、CND配置

#### 配置1：加速域名

-添加需要启用加速的域名

因此子域名可能会采用真实IP，子域名有可能和主站是相同的IP。

```plain
（www.xiaodi8.com、*.xioadi8.com）  192.168.1.5
```

#### 配置2：加速区域

-需要启用加速的地区（中国，外国等）

有些在国外是未启用CDN。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647642-100b0e07-0101-4857-aaa3-a0c742284569.png)

#### 配置3：加速类型

-需要启用加速的资源（图片、小文件、全站）

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647707-c270e55d-e219-4087-994b-ec74789f8378.png)

可以通过黑暗搜索引擎搜索。

超级工具国外地址较少：使用IPIP网站去ping。

网站：

https://tools.ipip.net/cdn.php

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647700-15b3fcb0-1ff4-4940-a37b-dc3d2a1cebb5.png)

## 一、CDN通过-SSRF漏洞&及遗留文件获取真实IP

### 1、SSRF漏洞利用

SSRF需要利用在存在允许远程上传的情况下才可以进行过去真实IP。

已测试网站http://www.yansiqi.com/ssrf.php为例，该网站存在允许远程。

python开启监听，利用SSRF访问python对外的地址，得到回显过去到真实IP。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647978-40020ac8-c0c9-4dfa-bca1-808622294126.png)

### 2、遗留文件

网站在PHP时会遗留下phpinfo.php文件，这个配置文件会泄露本身IP地址，将本地的地址给到你。php脚本主动把IP返回，如果返回的是对外的ip就可以获取到服务器的，内网开启IP则会获取到内网的IP。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647948-78ceffb1-bf49-457a-a892-4632d469f350.png)

## 二、CDN绕过-子域名查询操作

```plain
子域名极有可能和主站保持同一个IP
```

通过子域名的解析指向 也有可能指向目标的同一个IP上。

使用工具对其子域名进行穷举

查找目标域名信息的方法有：

```plain
\1.    FOFA title="公司名称"

\2.    百度 intitle=公司名称

\3.    Google intitle=公司名称 

\4.    站长之家，直接搜索名称或者网站域名即可查看相关信息：

http://tool.chinaz.com/

\5.    钟馗之眼 site=域名即可

https://www.zoomeye.org/
```

找到官网后，再收集子域名，下面推荐几种子域名收集的方法，直接输入domain即可查询。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648048-e7942319-31b5-4e15-9009-407ff7da83da.png)

## 三、CDN绕过-接口查询国外访问

网站：

https://tools.ipip.net/cdn.php

没有CDN：
)
![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345647996-2116549c-fe26-4d6d-a4f3-457072083dc5.png)

```plain
配置的区域可能只是中国大陆，在国外访问就是真是IP
国外访问：www.xuexila.com 119.132.147.27
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648066-d8d5c510-57f7-42e2-b8cc-48c6c1d7a1e3.png)

有些CDN厂家加速会划分到亚洲，欧州等区域。

## 四、CDN绕过-主动邮件配合备案

```plain
很多站点都有发送邮件sendmail的功能，如Rss邮件订阅等。而且一般的邮件系统很多都是在内部，没有经过CDN的解析。可在邮件源码里面就会包含服务器的真实 IP。是一个主动连接。
怎么样网站发邮箱给你：
邮箱找回验证码，显示邮件原文 219.153.49.169
大概率，不是百分百真实IP
如果又不确定，就进行社工判定，查看归属地
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648336-a6cd4fd5-83cf-44d1-898c-fe5cede51934.png)

下面是开启代理后发送的ip，判断为非官网IP。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648527-50ba08a4-49f0-4674-958e-6a7beca35583.png)

通过备案的地址判断。

## 五、互联网的扫描判断

1、https://get-site-ip.com/

网站判断ip，非一定真实，需要自己判断。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648561-0031a63a-376d-4ade-8db6-e55ca2870090.png)

2、Censys工具就能实现对整个互联网的扫描，Censys是一款用以搜索联网设备信息的新型搜索引擎，能够扫描整个互联网，Censys会将互联网所有的ip进行扫面和连接，以及证书探测。

若目标站点有https证书，并且默认虚拟主机配了https证书，我们就可以找所有目标站点

网址：

https://search.censys.io/search?resource=hosts

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648501-c22c99bf-2cd3-408f-98e2-e1143b16e466.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648796-c0a98eee-193c-43cc-9558-4db4c7905467.png)

修改hosts文件，域名指向地址，匹配对应真实IP地址

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649309-95624dc5-7f9f-47c0-ba0d-7ed7728a340b.png)

## 六、 dns历史绑定记录

通过以下这些网站可以访问dns的解析，有可能存在未有绑cdn之前的记录。

https://dnsdb.io/zh-cn/ ###DNS查询

https://x.threatbook.cn/ ###微步在线

http://viewdns.info/ ###DNS、IP等查询

https://tools.ipip.net/cdn.php ###CDN查询IP

https://sitereport.netcraft.com/?url=域名

![img](https://cdn.nlark.com/yuque/0/2024/gif/1591503/1720345648808-4a3496fe-255e-4c96-b37a-ecd79c2d4ac4.gif)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648810-a528681e-2ec2-4879-8e20-802561add0f1.png)

查询WWW.T00Ls.net的历史记录

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345648931-82b93a51-2c43-4807-a6d1-710ddaa419bb.png)

https://site.ip138.com/www.t00ls.net/

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649012-33480449-ee95-44fc-8916-4f2e956480a6.png)

## 七、CDN绕过-全网扫描FuckCDN

三个工具都可

https://github.com/superfish9/hackcdn

https://github.com/boy-hack/w8fuckcdn

https://github.com/Tai7sy/fuckcdn

fuckcdn：

### 首先设置set.ini

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649076-ffc746c6-e56a-42f6-8803-f32c84060a0e.png)

### 配置ip.txt文件：

这个文件中是填入服务器可能存在的ip段，可以填入所有的，相对应的扫描时常会久。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649082-8c78886e-6666-4971-8128-3dde7aec2efd.png)https://www.ipip.net/ip.html![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649540-06d07c21-11a5-4b3a-bc3f-48647cbc11a4.png)

```plain
两个文件完成后，运行输入：访问的cdn节点IP+端口
完成后在result_ip.txt文件中会出现IP。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649551-cb418fc0-43ed-4e28-873e-8211d257f3d4.png)

## 八、 ico图标通过空间搜索找真实ip

https://www.t00ls.net/favicon.ico 下载图标 放到fofa识别

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345649366-ac9aefb6-f577-4657-9cb4-75cb86e862ba.png)

通过fofa搜图标

![img](https://cdn.nlark.com/yuque/0/2024/gif/1591503/1720345649435-50095297-2b29-4a6f-b7d2-3ee08b13599e.gif)

通过这样查询 快速定位资源 查看端口是否开放 这里没有开放



来自: [第九天：信息打点-CDN绕过篇&；漏洞回链&；接口探针&；全网扫描&；反向邮件_站长之家cdn检测接口-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134916549?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113826483-51578747-c407-4c83-b5d4-8bfe1cb40bbc.png)

```plain
#知识点：
0、CDN知识-工作原理及阻碍
1、CDN配置-域名&区域&类型
2、CDN绕过-靠谱十余种技战法
3、CDN绑定-HOSTS绑定指向访问
hosts路径：C:\Windows\System32\drivers\etc
#前置知识：
1.传统访问（没有CDN）：用户访问域名–>解析服务器IP–>访问目标主机
2.普通CDN：用户访问域名–>CDN节点–>真实服务器IP–>访问目标主机
3.带WAF的CDN：用户访问域名–>CDN节点（WAF）–>真实服务器IP–>访问目标主机

#什么是CDN
CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821623-715d1b70-da29-40d7-ad63-53d0b5fa0708.png)

```plain
#CDN配置：
配置1：加速域名-需要启用加速的域名（www.xiaodi8.com、*.xioadi8.com）
配置2：加速区域-需要启用加速的地区（中国，外国等）
配置3：加速类型-需要启用加速的资源（图片、小文件、全站）

#判定标准：
nslookup,各地ping（出现多个IP即启用CDN服务）

#参考知识：
https://zhuanlan.zhihu.com/p/33440472
https://www.cnblogs.com/blacksunny/p/5771827.html
子域名，去掉www，邮件服务器，国外访问，证书查询，APP抓包
黑暗空间引擎，通过漏洞或泄露获取，扫全网，以量打量，第三方接口查询等

#案例资源：
超级Ping：https://www.17ce.com/
接口查询：https://get-site-ip.com/
国外请求：https://tools.ipip.net/cdn.php
全网扫描：https://github.com/Tai7sy/fuckcdn
```

- 真实应用-CDN绕过-漏洞&遗留文件

```plain
历史记录：
去年的时候没有开通，但是今年就有开通。
基于:主动连接
遗留文件：
网站下面含有phpinfo.php文件
www.yansiqi.com/phpinfo.php
这个配置文件会泄露本身IP地址，将本地的地址给到你。php脚本主动把IP返回。

漏洞：
http://www.yansiqi.com/ssrf.php
输入地址，服务器自动会请求这个地址 http://47.94.236.117:8000 这个
47.94.236.117运行：python2 -m SimpleHTTPServer 8000
47.94.236.117开通了web服务，记录日志 www.yansiqi.com 存在ssrf漏洞（会接受用户的数据并利用服务器去请求）
漏洞请求47.94.236.117日志就会记录访问IP
ping去请求网站，你自己的请求。所以cdn节点 对方自己的服务器请求你的IP，就会显示IP
```

- 真实应用-CDN绕过-子域名查询操作

```plain
加速域名上面导致的问题：如果设置是www.yansiqi.com，这样只加速www这个域名 如果有test.yansiqi.com 没有加速，是真是IP。一般子域名极有可能和主站保持同一个IP

https://www.sp910.com/
www.sp910.com
假如这个加速域名设置：www.sp910.com 加速
我们访问sp910.com  没加速
ping sp910.com这样就会显示出真是IP 119.23.136.136
ping www.sp910.com 不会显示真是IP

ping www.sp910.com:
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821144-85e5ad90-a337-4c9a-9bf9-3aebc08997bf.png)

ping sp910.com：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821124-3a2730c3-e3cc-49a8-b3bc-7d8c8476f8aa.png)

- 真实应用-CDN绕过-接口查询国外访问

```plain
配置的区域可能只是中国大陆，在国外访问就是真是IP
国外访问：www.xuexila.com 119.132.147.27
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821189-e3f199cc-1213-4c43-abd0-aa2567b5e617.png)

```plain
国内访问：www.xuexila.com 180.76.199.46
经过再次查找，找不到了：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821805-68e1a008-92bc-49bc-b2d5-c5714d46cdf3.png)

- 真实应用-CDN绕过-主动邮件配合备案

```plain
很多站点都有发送邮件sendmail的功能，如Rss邮件订阅等。而且一般的邮件系统很多都是在内部，没有经过CDN的解析。可在邮件源码里面就会包含服务器的真实 IP。是一个主动连接。

mozhe.cn
怎么样网站发邮箱给你：
邮箱找回验证码，显示邮件原文 219.153.49.169
大概率，不是百分百真是IP
如果又不确定，就进行社工判定，查看归属地
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821877-8a0e6b0b-456f-4d56-bb02-20c1f7092326.png)

- 真实应用-CDN绕过-全网扫描FuckCDN

```plain
资源加速问题：只是加速图片或者其他的，没有加速全站

运行fuckcdn.exe，设置ip.txt set.ini两个文件，运行输入访问的cdn节点IP+端口

配置set.ini文件：设置目标，findstr是设置关键字。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821695-1ab7b05b-9325-469d-b7ea-f60f4e52845a.png)

配置ip.txt：设置IP集合，https://www.ipip.net/ip.html

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113821982-8013c0a6-ead3-4809-9c9d-00d71d7c61d6.png)

完成后出现result_ip.txt文件。



来自: [安全的小菜鸡 - 第9天：信息打点-CDN绕过篇&漏洞回链&接口探针&全网扫描&反向邮件](http://www.mumuxi8.com/?id=12)
