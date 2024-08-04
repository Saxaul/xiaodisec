## 第6天：信息打点-Web架构篇&域名&语言&中间件&数据库&系统&源码![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442607-52ec879d-98b5-4436-9769-ebedc05817f3.png)

### #知识点：

```plain
1、打点-Web架构-语言&中间件&数据库&系统等
2、打点-Web源码-CMS开源&闭源售卖&自主研发等
开源：可以上网搜索，免费使用，使用人不太重视安全，例如博客是没必要使用开发的。
闭源：在内部，或者需要购买（源代码可能加密等）
自主研发：这套源码只是自己研发，只有自己用，不想让别人知道。（大型网站）
3、打点-Web源码获取-泄露安全&资源监控&其他等
4、打点-Web域名-子域名&相似域名&反查域名&旁注等

#信息点：收集的信息
基础信息，系统信息，应用信息，防护信息，人员信息，其他信息（子公司等）等

#技术点：
CMS识别，端口扫描，CDN绕过，源码获取，子域名查询，WAF识别，负载均衡识别等
```

## 一、信息打点-个人博客-XIAODI8-架构&源码

### 1.数据包分析

访问网站抓取数据包：分析数据：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345441750-591bac4e-ca32-4fa8-9aba-43e6fb69e799.png)

xiaodi8.com
审查元素
Apache 中间件

php 脚本语言

Z-BlogPHP cms源码名称

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345441917-0143b180-71ca-460e-80dc-98ecb21e2384.png)

```plain
判断操作系统：Windows    操作系统（大小写后缀判断，不要在参数加，在文件中测试）

在文件名判断操作系统也可以通过ping命令
Ping命令测试：根据ttl值来判定
```

数据判断： mysql , 通过搭建组合来判断猜测。

这边使用的apache百度搜索和什么数据库搭配：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345441914-1620e2f5-8890-48e9-9ba6-2bf5ec51ecba.png)

### 2.端口判断

在线扫描端口

```plain
https://www.cnblogs.com/ziyeqingshang/p/3769542.html
数据库access mysql mssql redis mongdb oracle postgresql
数据库常见开放端口https://blog.csdn.net/SeniorShen/article/details/108643979
接口：（被动扫描）网上平台 coolaf.com/tool/port
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345441917-9a678df7-d7a9-4db8-8b57-63e2d0692537.png)

### 3.信息利用

```plain
直接利用公开的漏洞进行尝试安全测试 白盒代码审计
mysql：漏洞或者弱口令等
PHP：常规的漏洞，如log4j漏洞
Apache：中间价安全等。
```

## 二、信息打点-某违法APP-面具约会-架构&源码

```plain
打开进行测试抓包（注册等）
发现host：love.youdingb.com
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442823-adab1bf4-f57d-4f5c-a5eb-57c88fe4ed8d.png)

```plain
返回包：发现Nginx，PHP，Linux，mysql
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442297-fe854d62-210f-4f71-b69e-0e2261a977ca.png)

复制地址访问：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442295-1a58778c-4dfd-4e37-b070-79b89568f424.png)

```plain
无法注册，没办法进行下一步测试。
mysql检测：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442344-147ce811-49c6-402b-9d20-5994b61f7eb4.png)

```plain
黑源码：关键字（棋牌源码）bing Google fofa上面搜索
https://www.huzhan.com/
https://28xin.com/
https://bbs.bcb5.com/
https://www.shixinwl.com/
https://www.lengcat.com/
https://www.xlymz.com/
https://www.ymadx.com/

利用fofa.so搜索：地址：https://www.i.youdingb.com/index.php/index/store/detail/id/74.html
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442714-4946ee68-d227-4156-8751-8a064748dcca.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442914-3bf43c09-bcb0-4c30-a2f7-ead0ca3e8ddf.png)

## 三、信息打点-某违法应用-爱心工程-架构&域名

```plain
http://jmlsd.com/
http://www.axgc168.com/
http://jmlsd.cn/
变动com 变成cn org cn.com 等
变动jmlsd 变成jmlsd123 jmlsd8等
找whois信息，备案信息，域名相似。后缀和名字
关键字：键镁乐时代
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345442657-65c58083-d03c-4e17-840a-135bcee05f7c.png)

可以利用到网上平台查询:

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345443085-86b123b1-2d75-46df-8a6b-80703b3742da.png)

## 四、信息打点-某专属SRC-补天平台-架构&域名

```plain
https://dxy.butian.net/ 
端口扫描，数据包返回 jsp3 
大型一般都是Linux
源码：圈内人或者其他
wangyuan.com
搜索为PHP信息
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345443028-9c5e7f1d-76ad-40fa-b456-dd15b54bcbea.png)

```plain
• 发现server 不懂百度。
• 这种情况下一般都会有cdn，需要找到真实IP。
全球ping,没有cdn
```





来自: [第6天：信息打点-Web架构篇&；域名&；语言&；中间件&；数据库&；系统&；源码_p6 第6天:信息打点-web架构篇&域名&语言&中间件&数据库&系统&源码获取 1:16:35-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134183568?spm=1001.2014.3001.5502)