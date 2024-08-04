\#知识点

原生JS开发框架-安全操作

常见安全问题-前端验证&未授权



\#详细点

1、什么时JS渗透测试

在JavaScript中也存在变量和函数，当存在可控变量及函数调用即可参数漏洞

JS开发的web应用和PHP、JAVA、NET等区别在于没有源码，也可以通过浏览器查看源代码获取真实的点，所以相当于JS开发的WEB应用属于白盒测试（默认有源码参考）

2、流行的JS框架有哪些

3、如何判定JS开发应用

插件wappalyzer

源代码简短

引入多个js文件

一般有/static/js/app.js等顺序的js文件

cookie中有connect.sid

4、如何获取更多的JS文件

Jafinder

packer-Fuzzer

扫描器后缀换字典

5、如何快速获取价值代码？

method：“get”

http.get(“

method：post”

http.post(“

$.ajax

sercice.httppost

service.httpget



\#演示案例

安全条件-可控变量&特定函数

开发框架-Vnlhub-Node.JS安全

补充学习：CVE-2017-14849漏洞复现

补充资料：https://vulhub.org/#/docs/

真实应用-APP应用直接重置密码

真实应用-违法彩彩文件上传



![img](https://cdn.nlark.com/yuque/0/2024/jpeg/1591503/1720339596428-efaf85b3-3b7c-4e75-a5f2-ab7a63e49a05.jpeg)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503456-28d27cef-5611-4e11-89d0-cde7994f334e.png)

```plain
#知识点：
1、原生JS&开发框架-安全条件
2、常见安全问题-前端验证&未授权

#详细点：
1、什么是JS渗透测试？
在Javascript中也存在变量和函数，当存在可控变量及函数调用即可参数漏洞
JS开发的WEB应用和PHP，JAVA,NET等区别在于即没有源代码，也可以通过浏览器的查看源代码获取真实的点。所以相当于JS开发的WEB应用属于白盒测试（默认有源码参考）

2、流行的Js框架有那些？
3、如何判定JS开发应用？
 插件wappalyzer
 源代码简短
 引入多个js文件
 一般有/static/js/app.js 等顺序的js文件
 cookie中有connect.sid
4、如何获取更多的JS文件？
 JsFinder
 Packer-Fuzzer
 扫描器后缀替换字典
5、如何快速获取价值代码？
 method:"get"
 http.get("
 method:"post"
 http.post("
 $.ajax
 service.httppost
 service.httpget
```

- 安全条件-可控变量&特定函数

本地代码演示

- 开发框架-Vulhub-Node.JS安全

```plain
来到相对应的目录，
docker-compose build 
docker-compose up -d 
config
```

- 真实应用-APP应用直接重置密码

```plain
用APP抓包，发现登录地址：mx2.simuwz.com/user/login
判断js:
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503661-9fac8d68-b05a-421c-9caf-def93677c9e3.png)

```plain
需要看js有没有嵌套js代码。
审查元素：关键性代码：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503155-c31f8c0d-00cf-4523-b9da-2cf217abf4d7.png)

```plain
未授权访问：
"/user/reset_password/" + e.phone
http://mx2.simuwz.com/user/reset_password/13554365566 ，就可以直接进入重置页面，但是会出现流程错误
关键判断：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503167-b36409fe-6dbf-4e05-8e63-82edb3836d72.png)

- 真实应用-违法彩彩文件上传安全

Ctrl+f 可以进行关键字搜索：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503163-767024bb-c3eb-4680-baa8-5ec5f64e71d1.png)

发现是前端验证代码：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503824-aa3150c3-ec19-4ff6-8727-9c1d6af98f68.png)

```plain
但是这个网站是js开发的，所以不能禁用js
用网站下载器可以去下载到源码，然后在进行禁用，但是后面下载不下来。
然后看上传视频可不可以。发现上传视频没有验证。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503803-1b37cad9-05db-4598-a755-93454a86035c.png)

抓包发现上传视频地址：但是访问无效

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721115503937-9ea42ff9-0c5f-4988-9e15-990698981378.png)

但是地址不解析PHP，不能连接。



来自: [安全的小菜鸡 - 第22天：WEB攻防-JS项目&Node.JS框架安全&识别审计&验证绕过](http://www.mumuxi8.com/?id=25)