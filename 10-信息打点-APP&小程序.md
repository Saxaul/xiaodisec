## 信息打点-APP&小程序

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345694680-8f32d141-9ed3-444f-b419-f9f80fe526c6.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345694685-8485e71a-fb90-4eb9-9127-4160942f6a71.png)

## 一、内在收集-代码

从app代码中去收集

### 1、移动端AppInfoScanner工具信息收集

安卓语法：

```plain
python app.py android -i <Your apk file>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695303-57b30a6d-44dc-463d-9658-0df2d3ca1ea6.png)

这个是从app代码中提取信息。

有些app会限制代理抓包，需要进行解壳。

类似CDN的技术，为你选择最佳的播放路径。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345694746-8971eb5c-2f42-4151-bf90-2cec32404699.png)

这里又获取到阿里云oss

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345694756-16f02cfe-faaa-4645-bcda-6b47c2085ea6.png)

### 2、安卓修改大师工具

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695873-46f2d884-7295-4ebd-a1bd-37b6039c985d.png)

反编译：![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695824-d3d827a9-1b8d-4c7a-8a13-8831f835cdef.png)**

从反编译的代码中去搜索关键信息例如：http：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695921-f9aedde2-7e5c-4b6b-b465-115a304ceaeb.png)

查找的结果不直观，可以通过idea进行代码搜索

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695318-f364ccd3-4040-45e2-b201-9995cc79b80e.png)

## 二、外在收集-抓包

工具：|茶杯，fd，burp

通过数据包获取app信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696963-dbc5d28d-3ab3-4df6-b358-08d9a39ae9c2.png)

## 三、资源提取-安装包&资源文件

抓不到数据包，通常是apk进行了加壳操作。

查询可以通过：apk查壳工具

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345695864-c1068024-227f-41c2-9a7a-1c2571d41f6d.png)

进行apk资源提取，常用工具：apk资源提取器，提取成功的资源可以进行fofa搜索相关信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696863-c90aae64-04a7-4c19-962a-d3c1203d869b.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696116-fcd52dc7-e5ff-4371-a494-41513eaf3057.png)

## 四、app无法抓包-Xposed & JustTrustMe 框架

[（转载）Android 神器 xposed 框架使用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/88028353)

xposed是一个框架，上面有很多模块,原理就是修改系统的关键文件，然后当APP调用系统API时，首先经过xposed

Xposed：用到这个就可以去壳，就可以进行进一步测试。绕过检测证书等进行抓包。
用到这个模块就可以忽略证书，如果单单用到fiddler或者burpsuite进行抓包，有些会检测证书，检测证书的话有些数据包就没办法抓。

## 五、微信-电脑版登录启动抓包分析

```plain
两个方面信息收集：web 和服务器
有些小程序其实就是APP 放到小程序里面
小程序管理后台：mituomimtm.cn/login
只能抓包，没有程序源码
```

小程序也是一样小程序是通过抓包，无法获取源代码# 信息打点-APP&小程序

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696275-6f4a4cc1-43de-49fd-8606-04b0a217cdf8.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696397-d2178e7b-b9e7-4137-966e-bd3c9f119a1f.png)

## 一、内在收集-代码

从app代码中去收集

### 1、移动端AppInfoScanner工具信息收集

安卓语法：

```plain
python app.py android -i <Your apk file>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345697269-9197756c-cdee-4d97-b6a2-dfe343414e28.png)

这个是从app代码中提取信息。

有些app会限制代理抓包，需要进行解壳。

类似CDN的技术，为你选择最佳的播放路径。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696704-fc24e4ae-1b55-4209-83c9-97535649e639.png)

这里又获取到阿里云oss

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345696909-50bc39ac-8270-4380-b6bf-d3917396d262.png)

### 2、安卓修改大师工具

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698170-aa78ceb3-2d03-4652-babf-d31dd7102504.png)

反编译：![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345697938-aa7ffc8e-57eb-428a-b1a3-4377efaadd07.png)**

从反编译的代码中去搜索关键信息例如：http：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698401-cef25931-89cf-4ebe-b2d7-eb9d2cf0ee0b.png)

查找的结果不直观，可以通过idea进行代码搜索

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345697938-5b2a4c3b-b5de-49c1-bd1f-152413bbfbcf.png)

## 二、外在收集-抓包

工具：|茶杯，fd，burp

通过数据包获取app信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698852-58a27ee4-1d43-4a01-bc44-d8d94d4e4b32.png)

## 三、资源提取-安装包&资源文件

抓不到数据包，通常是apk进行了加壳操作。

查询可以通过：apk查壳工具

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698524-c4a03ec2-7344-4b4a-b67d-b84b92947870.png)

进行apk资源提取，常用工具：apk资源提取器，提取成功的资源可以进行fofa搜索相关信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698841-ef40ec69-03e5-4f36-959d-6a0f0a039a2d.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345698554-03f0c181-442f-4b6a-aaee-d08f9386728b.png)

## 四、app无法抓包-Xposed & JustTrustMe 框架

[（转载）Android 神器 xposed 框架使用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/88028353)

xposed是一个框架，上面有很多模块,原理就是修改系统的关键文件，然后当APP调用系统API时，首先经过xposed

Xposed：用到这个就可以去壳，就可以进行进一步测试。绕过检测证书等进行抓包。
用到这个模块就可以忽略证书，如果单单用到fiddler或者burpsuite进行抓包，有些会检测证书，检测证书的话有些数据包就没办法抓。

## 五、微信-电脑版登录启动抓包分析

```plain
两个方面信息收集：web 和服务器
有些小程序其实就是APP 放到小程序里面
小程序管理后台：mituomimtm.cn/login
只能抓包，没有程序源码
```

小程序也是一样小程序是通过抓包，无法获取源代码



来自: [第十天：信息打点-APP&；小程序篇&；抓包封包&；XP框架&；反编译&；资产提取_支付宝小程序抓包-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/135437097?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114042388-13fb8d70-51f4-4cbe-8512-f3601fb8e084.png)

```plain
#知识点：
1.小程序-外在-资产收集

2.APP-外在&内在-资产收集

APP安卓入手
1.外在-抓包封包-资产安全测试
 资产收集-资源收取-ICO MD5,HASH-黑暗引擎搜索相关资产
1.1外在-功能逻辑

2.内在-反编译-Java代码-代码审计
2.1内在-反编译-提取RES资源-黑暗引擎收集
```

- APP-外在抓包-Fd&茶杯&Burp

```plain
Fd:
配置安卓代理等，找到想相对应的地址（图片地址101.32.39.61）
APP在加载的时候加载这个地址，并不是内在的地址。
从源代码分析到的资源，不是直接表现，在外在抓包进行分析。优先线路，加载对应资源。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114040785-22cf88dd-eb4f-4903-87be-caf3ff0b20f2.png)

发现PHP版本：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114039180-376bab2b-3fff-40b3-8dec-885b02e9489c.png)

- APP-外在封包-封包监听工具

```plain
如果抓包进行不了的话，那么就进行封包处理。这样也能进行资源的获取。也会抓到其他协议的数据包。逍遥模拟器不支持。

看发送数据，看到了 相对应的ID发送（手机机型）
修改手机机型，就可以更换观影次数。
```

- APP-内在提取-AppInfoScanner

```plain
把apk文件当前目录 python310 app.py android -i apk名称 

找到关键地址guifei2020.net 等关键网站
如果一些APP反编译不了，那么就需要去壳操作。把apk的资源提取出来后，用到黑暗引擎用来查找相关的资源。
比如猎豹.apk进行提取的话，那么就会报错（因为有加壳，反编译不了）
安装python310 -m pip install -r requirements.txt：解决不了问题
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114039150-218bb9b6-5e93-41eb-8dee-65b10a6f1b3b.png)

- APP-内在搜索-反编译载入IDEA

用安卓修改大师来进行反编译，用idea进行对源代码反编译，找到相对应的关键网站

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114039377-8451c865-427c-4739-a52c-216d64f24232.png)

反编译出来的源码来对应APP的功能：今日观看次数等。修改后在进行打包，这样就可以进行修改apk修改，达到修改资源的目的。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114039755-15bfc2d1-95af-4475-8394-118349e9d11b.png)

- APP-资源提取-安装包&资源文件

进行apk反编译获取源代码进行代码审计，找到相对应代码段，提取相对应的资源。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114040054-7ba9b904-f247-4848-a083-4ab4349546c8.png)

提取apk后，进行是否有壳测试：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114040088-9f4e05e1-c1f9-405e-abdb-0b5f1a70b064.png)

提取相对应的md5值和hash值，进行fofa搜索，找到相对应的资源。

- APP-框架使用-Xposed&JustTrustMe

```plain
Xposed：
用到这个就可以去壳，就可以进行进一步测试。绕过检测证书等进行抓包。
用到这个模块就可以忽略证书，如果单单用到fiddler或者burpsuite进行抓包，有些会检测证书，检测证书的话有些数据包就没办法抓。
```

- 小程序-微信-电脑版登录启动抓包分析

```plain
两个方面信息收集：web 和服务器
有些小程序其实就是APP 放到小程序里面
小程序管理后台：mituomimtm.cn/login
只能抓包，没有程序源码
```





来自: [安全的小菜鸡 - 第10天：信息打点-APP&小程序篇&抓包封包&XP框架&反编译&资产提取](http://www.mumuxi8.com/?id=13)