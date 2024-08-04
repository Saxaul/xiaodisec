```plain
1、抓包技术应用意义

//有些应用或者目标是看不到的，这时候就要进行抓包
2、抓包技术应用对象 //app,小程序
3、抓包技术应用协议 //http，socket
4、抓包技术应用支持
5、封包技术应用意义

总结点：学会不同对象采用不同抓包封包抓取技术分析
对象：应用，网站，小程序，桌面应用
```

## 一、基于网络接口抓包-网络接口

```plain
问xiaodi8.com/zb_system/login.php，浏览器查看元素就可以进行数据包的抓取，或者使用burpsuite：
检查元素：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174517-82411a16-98e6-4acf-98f5-f0adc87d4cd4.png)

## 二、APP&小程序&PC抓包HTTP/S数据-Charles&Fiddler&Burpsuite

```plain
#环境配置：
1、安卓模拟器安装搭建
逍遥，雷电，夜神等自行百度下载安装
2、工具相关证书安装指南 安装证书才能访问https网站 非web协议
Charles 配置代理与burpsuite一样
https://blog.csdn.net/weixin_45459427/article/details/108393878
Fidder
https://blog.csdn.net/weixin_45043349/article/details/120088449
BurpSuite
https://blog.csdn.net/qq_36658099/article/details/81487491
三款工具都需要安装证书。这样才能抓取https
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345173872-6db5e2a5-565a-4423-a8a9-7dc9923be6cd.png)

## 代理设置：

### 茶杯

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174068-9f315535-fd47-4f00-bc95-4be7d997a523.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345173862-6c2c7fb9-0d72-437c-a637-de7704e26f64.png)

### 手机端：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345173938-561e4b46-3227-4840-82b6-8c20406c384c.png)

### Fidder

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174490-f9828744-f4ea-4020-84f3-0e22d172a6dd.png)

注释：这三款软件只能抓取web协议的数据包

测试网易有道词典，抓包没有发现相关数据包

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174665-f1e18d4c-6494-4589-8fc7-f0aa1f23853f.png)

```plain
burpsuite，茶杯，fiddler
• 模拟器设置证书后
• 设置代理->运行工具的本机IP 端口
• Burpsuite 茶杯 fiddler 配置代理监听抓取
```

## 三、基于其他协议抓包-工具：WireShark&科来网络分析系统

### 1.Wireshark

```plain
非web协议。
Wireshark: 能抓到其他协议。
```

数据包十分庞大，要自己过滤

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174452-42482d60-e623-41fd-b60d-bb090fa0a6c2.png)

### 2.科来

直接抓取网卡数据包

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174473-531497fb-4069-445f-b494-708bf9c01e56.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345174871-07442cd5-7101-438c-b879-49353a85e901.png)

页面更清晰直观

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345175302-65d25cfe-6847-4780-9974-32ce6d2193ad.png)

```plain
Wireshark 科来 不需要配置任何东西，抓的是网络接口
```

## 四、app通讯类应用封包分析发送接收-WPE四件套封包

app中：有些数据不会走web协议，这时候应该怎么办

抓包和封包不同的技术。

### 1.获取数据

```plain
• 科来分析：
• 移动任务，tcp一直建立联系，一直有数据交互，确保通讯，建立连接。 
• 交互地址81.69.41.108
游戏移动人物，点击回城进行封包：选择相对应的进程，
```

这里走的是TCP协议，这里获取到了IP就可以进行测试了

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345175837-54b7d6a1-e649-412e-9173-6d402d68b9c4.png)

### 2.封包技术

```plain
封包技术就是：抓到的数据包（零散的数据包）进行整个封装。
```

#### 工具：封包监听工具2.3

下载地址

[【新提醒】x64封包监听工具2.3（可抓模拟器等64位进程游戏） - 免费工具更新下载 - 下雪论坛 - Powered by Discuz! (xiaxueluntan.com)](http://www.xiaxueluntan.com/forum.php?mod=viewthread&tid=465)

通过封包这个软件可以抓到安卓模拟器中的所有的请求（小程序和pc应用不可以）
1.打开逍遥安卓模拟器，下载并安装任意游戏，并在设置里关闭WLAN的代理
2.启动封包.exe，取消接受包和封包大小限制，选择对应的进程即可抓取到所有的数据

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345176116-9df76e8b-106f-4b6d-b068-e38d97eb885e.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345176045-9a4120e4-93ab-4272-bee4-76bfe877612f.png)

3.我们可以尝试利用抓取到的数据，首先清空所有的抓包数据，然后在游戏里开启瞬移操作，可以看到数据包的长度发生了变化，此时我选中该数据包，点击发送按钮，即可在游戏里再次瞬移，对于安全测试来说，远程ip端口是我们首要关注的信息

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345175507-8e93de16-4faa-4a88-81d2-3bb391dd4728.png)

#### 工具：ccproxy(遥志代理服务器)结合wpe

老牌子工具了

这个工具可以解决上面工具不能解决的一些问题

设置代理端口

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345175845-920a5101-44cd-4efe-919a-8630bd3563ed.png)

过程：

```plain
Apk-->给到ccproxy-->给到wpe 监控cc进行，实现封包抓包
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345175888-1ab5a93d-4635-4fda-855c-b08efb17aab5.png)



来自: [第3天：基础入门-抓包&；封包&；协议&；APP&；小程序&；PC应用&；WEB应用_封包监听工具2.3-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134141149?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113099701-6eddc830-a682-485e-b02a-b672a80fb74e.png)

```plain
#知识点：
1、抓包技术应用意义 //有些应用或者目标是看不到的，这时候就要进行抓包
2、抓包技术应用对象 //app,小程序
3、抓包技术应用协议 //http，socket
4、抓包技术应用支持
5、封包技术应用意义
总结点：学会不同对象采用不同抓包封包抓取技术分析
对象：应用，网站，小程序，桌面应用

基于网络接口抓包-网络接口
基于程序进程抓包-程序进程
基于数据协议抓包-HTTP/S&TCP&UDP
基于应用对象抓包-APP&小程序&PC_UI
基于系统使用抓包-模拟器&WIN&LINUX
基于应用对象封包-WPE动作数据包重放通讯

#参考点：抓包工具，面向不同，选取不同工具
Fiddler：http
是一个http协议调试代理工具，它能够记录并检查所有你的电脑和互联网之间的http通讯，设置断点，查看所有的“进出”Fiddler的数据（指cookie,html,js,css等文件）。 Fiddler 要比其他的网络调试器要更加简单，因为它不仅仅暴露http通讯还提供了一个用户友好的格式。

Charles：http
是一个HTTP代理服务器,HTTP监视器,反转代理服务器，当浏览器连接Charles的代理访问互联网时，Charles可以监控浏览器发送和接收的所有数据。它允许一个开发者查看所有连接互联网的HTTP通信，这些包括request, response和HTTP headers （包含cookies与caching信息）。

TCPDump：
是可以将网络中传送的数据包完全截获下来提供分析。它支持针对网络层、协议、主机、网络或端口的过滤，并提供and、or、not等逻辑语句来帮助你去掉无用的信息。

BurpSuite：http
是用于攻击web 应用程序的集成平台，包含了许多工具。Burp Suite为这些工具设计了许多接口，以加快攻击应用程序的过程。所有工具都共享一个请求，并能处理对应的HTTP 消息、持久性、认证、代理、日志、警报。

Wireshark：所有协议
是一个网络封包分析软件。网络封包分析软件的功能是截取网络封包，并尽可能显示出最为详细的网络封包资料。Wireshark使用WinPCAP作为接口，直接与网卡进行数据报文交换。

科来网络分析系统：所有协议
是一款由科来软件全自主研发，并拥有全部知识产品的网络分析产品。该系统具有行业领先的专家分析技术，通过捕获并分析网络中传输的底层数据包，对网络故障、网络安全以及网络性能进行全面分析，从而快速排查网络中出现或潜在的故障、安全及性能问题。

WPE&封包分析：
是强大的网络封包编辑器，wpe可以截取网络上的信息，修改封包数据，是外挂制作的常用工具。一般在安全测试中可用来调试数据通讯地址。
#环境配置：
1、安卓模拟器安装搭建
逍遥，雷电，夜神等自行百度下载安装
2、工具相关证书安装指南 安装证书才能访问https网站 非web协议
Charles 配置代理与burpsuite一样
https://blog.csdn.net/weixin_45459427/article/details/108393878
Fidder
https://blog.csdn.net/weixin_45043349/article/details/120088449
BurpSuite
https://blog.csdn.net/qq_36658099/article/details/81487491
3、封包抓取调试见课程操作
直接浏览器访问跟app访问不一样

非http其他协议需要用到WireShark&科来网络分析系统工具
通讯类的游戏是建立连接，要一直建立连接，这样数据就不会掉线。因此用到tcp协议。找到通讯ip

封包技术 找到相对应的进程 抓包：零散的 封包：一个操作

apk--ccproxy--wpe监控cc进程 实现封包抓包
burpsuite 茶杯 fiddler 
模拟机设置蒸熟后
设置代理->运行攻击的本机ip 端口

wireshark 科来 不需要配置任何东西
抓网络接口

1.为什么要抓包
抓包应用的资产信息进行安全测试
2.抓包对象有哪些
小程序，APP，桌面应用
3.抓包协议区别工具
有部分应用不走http 需要用到全局协议抓包
4.封包抓包不同之处
零散整体的区别，封包能精确到每个操作的数据包。（一段数据包）
```

- WEB应用站点操作数据抓包-浏览器审查查看元素网络监听

```plain
• 访问xiaodi8.com/zb_system/login.php，浏览器查看元素就可以进行数据包的抓取，或者使用burpsuite：
检查元素：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113097164-15487832-6b41-401a-9fd0-1fe40f9b5ca0.png)

burpsuite：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113096998-74f53eb6-490a-4633-bb7c-ae8a7033d76d.png)

- APP&小程序&PC抓包HTTP/S数据-Charles&Fiddler&Burpsuite

```plain
• 三款工具都需要安装证书。这样才能抓取https
• APP:
启动贵妃 配置Charles的抓包代理：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113099051-111aaca5-8ecf-4551-a30a-8402c08ba932.png)

```plain
抓到数据包101.34.24.242
茶杯：展示比较有条理，但是数据包过多就不容易筛选。

Fiddler:数据包抓取配置网上可以直接查。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113097344-29f0be9c-2e13-40ac-8fce-66114d9d582b.png)

```plain
访问：101.34.39.61/uploads/cover/2020/04/03/af6a64cbdc8dfa1140df7d03e353fa68.jpg 就可以直接得到图片地址。
数据包：
GET /uploads/cover_vertical/2020/04/27/2c6c819f68bc5e5dbc0b5ae3f8c6fdd6.jpg HTTP/1.1
User-Agent: Dalvik/2.1.0 (Linux; U; Android 7.1.2; HD1910 Build/N2G48H)
Host: 101.32.39.61
Connection: Keep-Alive
Accept-Encoding: gzip

burpsuite：能够实时的，点对点的抓包。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113097630-6a03d13e-f388-4fa3-8279-0fbc2da8a3ff.png)

```plain
数据包：
POST /new_market/service.php HTTP/1.1
Content-Type: application/json;charset=UTF-8
Content-Length: 313
Host: stat.microvirt.com
Connection: close
Accept-Encoding: gzip, deflate
User-Agent: okhttp/3.10.0

{"appFrom":"-1","appId":"-1","appName":"桂妃","module":"launcher-desktop","op":"click","packageAppName":"net.ufozfnxzqm.dvbphwfo","position":"-1","resourceType":"5","action":"postpcmanagementop","channelId":"7c8a454c","mac":"00:E0:4C:8B:95:30","marketVersion":"launcher_5.9.1","userName":"-1","version":"7.3.2"}
小程序：
用burpsuite抓不到数据包的。
用到茶杯抓包：
因为电脑打开不了小程序，所以不能做实验。
fiddler一样原理。
PC程序：抓到腾讯文档数据包
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113097922-54f1b2fe-12dc-4099-8636-eb8f2acbdec9.png)

```plain
CONNECT docs.qq.com:443 HTTP/1.1
Host: docs.qq.com:443
Proxy-Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) TDAppDesktop/2.2.26 Chrome/91.0.4472.164 Electron/13.3.0 Safari/537.36
```

- 程序进程&网络接口&其他协议抓包-WireShark&科来网络分析系统

```plain
• 非web协议。
Wireshark: 能抓到其他协议。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113099470-fb5b0dce-7623-4062-8540-f848ca8b98d8.png)

科来：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113099308-af33b955-1338-409e-a454-6397080ece6e.png)

科来可以根据进程，协议对数据包进行分析和区分开来，比较合适。

- 通讯类应用封包分析发送接收-WPE四件套封包&科来网络分析系统

```plain
• 永恒沉默：有些数据不会走web协议，这时候应该怎么办
• 科来分析：
• 移动任务，tcp一直建立联系，一直有数据交互，确保通讯，建立连接。 
• 交互地址81.69.41.108
游戏移动人物，点击回城进行封包：选择相对应的进程，
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113099017-d112372d-e13c-4e05-bca0-5b58420b8a90.png)

```plain
• 去抓到的数据包（零散的数据包）进行整个封装。
• WEP封包技术：
• 把代理机器人放去安卓模拟器，然后在利用ccproxy.exe监听代理机器人，最后用到WPE工具监听ccproxy.exe进程。完成测试。
• 演示不成功。

• Apk-->ccproxy-->wpe 监控cc进行，实现封包抓包。
• 
• burpsuite，茶杯，fiddler
• 模拟器设置证书后
• 设置代理->运行工具的本机IP 端口
• Burpsuite 茶杯 fiddler 配置代理监听抓取

Wireshark 科来 不需要配置任何东西，抓的是网络接口
```





来自: [安全的小菜鸡 - 第3天：基础入门-抓包&封包&协议&APP&小程序&PC应用&WEB应用](http://www.mumuxi8.com/?id=6)