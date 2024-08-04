## 第二天：基础入门-Web应用&架构搭建&漏洞&HTTP数据包&代理服务器

### ![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076007-ef8f62e9-3ba5-4789-a374-4525dd4d86c8.png)#知识点：

```plain
1、网站搭建前置知识
2、WEB应用环境架构类
3、WEB应用安全漏洞分类
4、WEB请求返回过程数据包
```

## 一、网站搭建前置知识

```plain
网站包括：开发语言、程序源码、中间件容器、数据库类型、服务器操作系统、第三方软件（如phpmyadmin,vs-ftpd,VNC,ELK,Openssh等）。

phpMyAdmin是基于PHP 开发的基于B/S模式的 管理MySQL 的软件。
```

#### **中间件容器**

```plain
IIs,Apache,Nginx,Tomcat,Weblogic,Jboos,glasshfish等.

不同的中间件对不同功能或不同源码的网站，支持性上的优势有大有小，所以会有Mysql数据库搭配php脚本的程序,SQlServer数据库搭配asp / aspx脚本、IIS多搭配ASP / ASPX；apache则搭配php居多。tomcat是jsp居多。
```

#### **WEB应用安全漏洞分类**

```plain
SQL注入，文件安全（包括文件上传、下载、删除等），RCE执行，XSS跨站，CSRF/SSRF/CRLF，反序列化，逻辑越权，未授权访问，XXE/XML,弱口令安全等。我们学的漏洞，大部分是产生于程序源码层面。
```

**WEB请求返回过程数据包格式**

```plain
HTTP协议通信过程：浏览器建立与web服务器之间的连接——>发送请求数据包——>返回响应数据包——>web服务器关闭连接
```

#### **请求数据包**

```plain
1.请求行：请求类型/请求资源路径、协议的版本和类型

2.请求头：一些键值对，一般由w3c定义，浏览器与web服务器之间都可以发送，表示特定的某种含义

Accept：指浏览器或其他客户可以接爱的MIME文件格式。Servlet可以根据它判断并返回适当的文件格式。

User-Agent：是客户浏览器名称

Host：对应网址URL中的Web名称和端口号。

Accept-Langeuage：指出浏览器可以接受的语言种类，如en或en-us，指英语。

connection：用来告诉服务器是否可以维持固定的HTTP连接。http是无连接的，HTTP/1.1使用Keep-Alive为默认值，这样，当浏览器需要多个文件时(比如一个HTML文件和相关的图形文件)，不需要每次都建立连接

Cookie：浏览器用这个属性向服务器发送Cookie。Cookie是在浏览器中寄存的小型数据体，它可以记载和服务器相关的用户信息，也可以用来实现会话功能。

Referer：表明产生请求的网页URL。如比从网页/icconcept/index.jsp中点击一个链接到网页/icwork/search，在向服务器发送的GET/icwork/search中的请求中，Referer是http://hostname:8080/icconcept/index.jsp。这个属性可以用来跟踪Web请求是从什么网站来的。

Content-Type：用来表名request的内容类型。可以用HttpServletRequest的getContentType()方法取得。

Accept-Charset：指出浏览器可以接受的字符编码。英文浏览器的默认值是ISO-8859-1.

Accept-Encoding：指出浏览器可以接受的编码方式。编码方式不同于文件格式，它是为了压缩文件并加速文件传递速度。浏览器在接收到Web响应之后先解码，然后再检查文件格式。

3.【空行】请求头与请求体之间用一个空行隔开；
```

#### 响应数据包

```plain
1.状态行：协议版本、数字形式的状态代码和状态描述，空格分隔

常见状态码

1**：提示信息-表示请求已收到，继续处理

2**：发送成功（200）

3**：重定向

301 状态码表明目标资源被永久的移动到了一个新的 URI，任何未来对这个资源的引用都应该使用新的 URI。

302 临时跳转，跳转的地址通过Location指定

4**：客户端错误

　　400.客户端请求有语法错误，不能被服务器识别

　　401.访问页面没有授权，侧重于身份未认证，例如用户没登陆账号，尝试访问管理员权限的api时

　　403.没有权限访问该页面，侧重于服务器资源不给看

　　404.没有该页面

5**：服务端错误

　　500.服务器内部异常

　　504.服务器请求超时，没有返回结果

2.响应头：包含服务器类型、日期、长度、内容类型等

3.【空行】响应头与响应体之间用空行隔开

4.响应正文：程序处理后果，浏览器会将实体内容中的数据取出来，生成相应的页面
```

## 二、服务器阿里云搭建

### 1.购买服务器

根据需求购买适合的服务器

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345075796-05ffd3df-62b0-409f-bac4-f97635d081b8.png)

地域中域名没备案选择国外的

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345075724-3d25cfb8-b201-4c20-bc4f-12e312885f94.png)

### 2.域名购买

在搭建服务器之前先要购买一个域名，阿里云购买网站：

[域名_域名查询_域名注册_.com_.cn-阿里云 (aliyun.com)](https://wanwang.aliyun.com/domain/tld#.com)

这里在国内没有经过备案，域名只能在国外解析。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076147-30e72218-35c2-43cd-a2b9-03ba812fec65.png)

后缀不同价格不同：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345075860-10cb688b-9f70-4374-a167-0817d943b047.png)

将域名添加到服务器。

## 请求包-新闻回帖点赞-重放数据包

```plain
设置代理，能进行包重发
新浪新闻：news.sina.com.cn
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076072-798a90d9-b4f4-4800-ac13-9dd0efc4ac5b.png)

```plain
点赞进行抓包，然后把抓到的包重放，就可以进行无限次点赞操作。
点赞数据包：发送到repeater模块，就可以进行重放，实验成功。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076215-2011d1cf-e50c-4719-94e8-cdfdbf8edc81.png)

```plain
抓不到https的数据包的时候，设置：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076176-d2fd48b5-6f77-416a-b857-b481e8b59a93.png)

- 请求包-移动端&PC访问-自定义UA头

```plain
安卓访问：
GET / HTTP/1.1
Host: www.baidu.com
User-Agent: Mozilla/5.0 (Android 7.1.2; Tablet; rv:68.0) Gecko/68.0 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: BAIDUID=582046B331C25B9F4A11725B4C4D814D:FG=1; H_WISE_SIDS=107318_110085_114550_127969_132547_164870_179345_182234_184716_186635_186743_186840_187828_188740_189254_189325_189732_189755_190248_190473_190621_191067_191250_191368_191736_191810_192010_192206_192237_192351_192387_193037_193284_193348_193559_194085_194520_194583_195038_195108_195188_195342_195423_195552_195577_195679_195756_195915_196046_196051_196230_196428_196513_196834_196925_196940_197241_197288_197294_197337_197470_197471_197582_197710_197783_197810_197845_198076_198181_198189_198243_198260_198312_198419_198512_198579_198583_198649_198748_198897_198997_199023_199232_199302_199467_199566_199675_199840_199847_199877_199968; Hm_lvt_12423ecbc0e2ca965d84259063d35238=1638805266,1639040042; rsv_i=85a28ee9x%2FywqxcJFJBh4a3wljQbeaE0my5E8hL5ATMrcrDBUFBOOIz4x%2Fy%2FS%2FeqdoqROV8cBAKDFCZ6vO5MTqJLFC86iu4; BDSVRTM=345; plus_lsv=e9e1d7eaf5c62da9; plus_cv=1::m:7.94e+147
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
显示页面：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076660-969d25e6-c35b-4047-9f1e-2a7070bb57a8.png)

```plain
pc访问：

GET / HTTP/1.1
Host: www.baidu.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Cookie: BAIDUID=8DCC4DBC473D921F92F028884ABD901C:FG=1; BIDUPSID=8DCC4DBC473D921F2292212FDC9EB8D6; PSTM=1638837611; BD_UPN=13314752; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_PSSID=35637_34442_35104_31254_35626_34584_35491_35234_35644_35320_26350_35478_35561; BA_HECTOR=0hah8ka12g0405003b1gt4moe0q; baikeVisitId=caf11396-3737-45ea-bd2d-03a92b598836; delPer=0; BD_CK_SAM=1; PSINO=6; BD_HOME=1
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1

显示界面：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076696-0b645095-5614-4209-81c2-be2fe43865e2.png)

```plain
两个访问的页面不一样，网站可以通过ua头来进行判断是手机访问，还是电脑访问。
电脑ua:
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
安卓模拟器ua：
User-Agent: Mozilla/5.0 (Android 7.1.2; Tablet; rv:68.0) Gecko/68.0 Firefox/68.0
修改au头后，完成测试：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076691-3890c226-a530-4739-8b96-4f32b19db557.png)

## 返回包-网站文件目录扫描-返回状态码

```plain
返回信息：

返回状态码：200，300,400,403,404,500
3XX跳转 处理过程，判断可有可无 
5XX内部错误，服务器问题，文件判断可有可无

复制图片地址：http://xiaodi8.com/zb_users/upload/2019/12/201912151576406028214564.jpg
抓包请求：找一个不存在的地址，返回404
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076707-6ed2d3b9-39e1-4a53-9b6a-b41224319ae6.png)

```plain
一个存在地址，返回：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076715-96c8c691-b222-4b73-bc68-c6fe21df9540.png)

```plain
可以根据对方放回的数据包的值来判断文件是否存在（状态码）
存在文件夹返回：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345076981-4a8f6422-64ec-48e2-a8f5-ce61e7b1dbe3.png)

```plain
不存在文件夹返回：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345077158-e66bdbc8-4d97-4b1d-aa52-6eef4e472b27.png)

```plain
目录扫描可以用到burpsuite，进行简单的扫描。
```

## 数据包-WAF文件目录扫描-代理服务器

```plain
• 代理服务器原理：
没设置代理
浏览器--服务器
设置代理
浏览器--代理--服务器
127.0.0.1 8888
burpsuite监听127.0.0.1 8888
burpsuite拦截后选择性的进行丢弃或者放行

购买代理：www.kuaidaili.com
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345077190-fa0b26e9-3bab-4a1d-925c-d491b85f942e.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345077317-3bf3c605-363b-45a3-ae2b-1d42a43815ff.png)



来自: [第二天：基础入门-Web应用&；架构搭建&；漏洞&；HTTP数据包&；代理服务器_web应用在阿里云上的基础架构-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134114787?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946526-87cb2e3c-f839-4671-943e-95ee95812341.png)

```plain
#知识点：
1、网站搭建前置知识
2、WEB应用环境架构类
3、WEB应用安全漏洞分类
4、WEB请求返回过程数据包
#WEB应用安全漏洞分类
SQL注入，文件安全，RCE执行，XSS跨站，CSRF/SSRF/CRLF，反序列化，逻辑越权，未授权访问，XXE/XML，弱口令安全等
产生层面要区分出来


#WEB请求返回过程数据包参考：
https://www.jianshu.com/p/558455228c43
https://www.cnblogs.com/cherrycui/p/10815465.html
请求数据包，请求方法，请求体，响应包，响应头，状态码，代理服务器等
Request,Response,User-Agent,Cookie,Server,Content-Length等

Accept：指浏览器或其他客户可以接爱的MIME文件格式。Servlet可以根据它判断并返回适当的文件格式。

User-Agent：是客户浏览器名称

Host：对应网址URL中的Web名称和端口号。

Accept-Langeuage：指出浏览器可以接受的语言种类，如en或en-us，指英语。

connection：用来告诉服务器是否可以维持固定的HTTP连接。http是无连接的，HTTP/1.1使用Keep-Alive为默认值，这样，当浏览器需要多个文件时(比如一个HTML文件和相关的图形文件)，不需要每次都建立连接

Cookie：浏览器用这个属性向服务器发送Cookie。Cookie是在浏览器中寄存的小型数据体，它可以记载和服务器相关的用户信息，也可以用来实现会话功能。

Referer：表明产生请求的网页URL。如比从网页/icconcept/index.jsp中点击一个链接到网页/icwork/search，在向服务器发送的GET/icwork/search中的请求中，Referer是http://hostname:8080/icconcept/index.jsp。这个属性可以用来跟踪Web请求是从什么网站来的。

Content-Type：用来表名request的内容类型。可以用HttpServletRequest的getContentType()方法取得。

Accept-Charset：指出浏览器可以接受的字符编码。英文浏览器的默认值是ISO-8859-1.

Accept-Encoding：指出浏览器可以接受的编码方式。编码方式不同于文件格式，它是为了压缩文件并加速文件传递速度。浏览器在接收到Web响应之后先解码，然后再检查文件格式。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946035-2b6d946d-2b70-4cd2-8bf3-4dde4d533baf.png)

```plain
POST /cmnt/vote HTTP/1.1
Host: comment.sina.com.cn
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: https://sports.sina.com.cn/others/volleyball/2022-01-01/doc-ikyakumx7803364.shtml
Cookie: SUB=_2AkMW7WMxf8NxqwFRmP0Qzmvka4h3ygDEieKgsZLqJRMyHRl-yD92qnEBtRB6PW1N3mpNlSc6uR7VjBSJygF1765GdUKX; SUBP=0033WrSXqPxfM72-Ws9jqgMF55529P9D9WW3n48F2PpVgV_avH7niNaF; 
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 139
```

- 架构-Web应用搭建-域名源码解析

```plain
购买服务器，进行宝塔下载安装。宝塔：集成化工具
安装Apache+PHP+mysql+PHPmyadmin
一键部署可以自己下载cms，比如zblog
有手就行。
```

- 请求包-新闻回帖点赞-重放数据包

```plain
设置代理，能进行包重发
新浪新闻：news.sina.com.cn
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946034-644130b6-0343-4efe-8e4a-43dd9d546258.png)

```plain
点赞进行抓包，然后把抓到的包重放，就可以进行无限次点赞操作。
点赞数据包：发送到repeater模块，就可以进行重放，实验成功。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946633-47afcc68-4435-43c2-aba8-90879aef5ddd.png)

抓不到https的数据包的时候，设置：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946110-2f164ef4-4c32-44ff-be15-933a8662afc7.png)

- 请求包-移动端&PC访问-自定义UA头

```plain
安卓访问：
GET / HTTP/1.1
Host: www.baidu.com
User-Agent: Mozilla/5.0 (Android 7.1.2; Tablet; rv:68.0) Gecko/68.0 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: BAIDUID=582046B331C25B9F4A11725B4C4D814D:FG=1; H_WISE_SIDS=107318_110085_114550_127969_132547_164870_179345_182234_184716_186635_186743_186840_187828_188740_189254_189325_189732_189755_190248_190473_190621_191067_191250_191368_191736_191810_192010_192206_192237_192351_192387_193037_193284_193348_193559_194085_194520_194583_195038_195108_195188_195342_195423_195552_195577_195679_195756_195915_196046_196051_196230_196428_196513_196834_196925_196940_197241_197288_197294_197337_197470_197471_197582_197710_197783_197810_197845_198076_198181_198189_198243_198260_198312_198419_198512_198579_198583_198649_198748_198897_198997_199023_199232_199302_199467_199566_199675_199840_199847_199877_199968; Hm_lvt_12423ecbc0e2ca965d84259063d35238=1638805266,1639040042; rsv_i=85a28ee9x%2FywqxcJFJBh4a3wljQbeaE0my5E8hL5ATMrcrDBUFBOOIz4x%2Fy%2FS%2FeqdoqROV8cBAKDFCZ6vO5MTqJLFC86iu4; BDSVRTM=345; plus_lsv=e9e1d7eaf5c62da9; plus_cv=1::m:7.94e+147
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
显示页面：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947234-56631c5d-28c6-4a88-9f58-c93a493a08a6.png)

```plain
pc访问：

GET / HTTP/1.1
Host: www.baidu.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Cookie: BAIDUID=8DCC4DBC473D921F92F028884ABD901C:FG=1; BIDUPSID=8DCC4DBC473D921F2292212FDC9EB8D6; PSTM=1638837611; BD_UPN=13314752; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_PSSID=35637_34442_35104_31254_35626_34584_35491_35234_35644_35320_26350_35478_35561; BA_HECTOR=0hah8ka12g0405003b1gt4moe0q; baikeVisitId=caf11396-3737-45ea-bd2d-03a92b598836; delPer=0; BD_CK_SAM=1; PSINO=6; BD_HOME=1
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1

显示界面：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946722-a5e1e282-69a6-4637-b664-7d04148e0bf6.png)

```plain
两个访问的页面不一样，网站可以通过ua头来进行判断是手机访问，还是电脑访问。
电脑ua:
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
安卓模拟器ua：
User-Agent: Mozilla/5.0 (Android 7.1.2; Tablet; rv:68.0) Gecko/68.0 Firefox/68.0
修改au头后，完成测试：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112946900-d6f8d4ed-a90a-4aa8-88cd-e2e8b8b8c221.png)

- 返回包-网站文件目录扫描-返回状态码

```plain
返回信息：

返回状态码：200，300,400,403,404,500
3XX跳转 处理过程，判断可有可无 
5XX内部错误，服务器问题，文件判断可有可无

复制图片地址：http://xiaodi8.com/zb_users/upload/2019/12/201912151576406028214564.jpg
抓包请求：找一个不存在的地址，返回404
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947392-c6b88d89-0ab1-4a73-be98-45f6c866244f.png)

一个存在地址，返回：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947451-e209f25e-1d5b-41ca-9758-c3e65f01d290.png)

```plain
可以根据对方放回的数据包的值来判断文件是否存在（状态码）
存在文件夹返回：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947714-3b6aa19b-4d1c-481f-9133-ef9b1cb752d3.png)

不存在文件夹返回：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947289-81841b22-8bab-41ac-a0b4-97a92fbe59eb.png)

目录扫描可以用到burpsuite，进行简单的扫描。

- 数据包-WAF文件目录扫描-代理服务器

```plain
• 代理服务器原理：
没设置代理
浏览器--服务器
设置代理
浏览器--代理--服务器
127.0.0.1 8888
burpsuite监听127.0.0.1 8888
burpsuite拦截后选择性的进行丢弃或者放行

购买代理：www.kuaidaili.com
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721112947711-7067e516-6336-43be-9327-59a283795586.png)