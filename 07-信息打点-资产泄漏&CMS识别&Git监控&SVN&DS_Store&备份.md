## 第7天：信息打点-资产泄漏&CMS识别&Git监控&SVN&DS_Store&备份

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113490797-044960b7-192c-4ba7-bec7-0fd1818e1cec.png)

## 知识点：

```plain
#知识点：
1.CMS指纹识别源码获取方式 源码获取方法
2.习惯&配置&特性等获取方式
3.托管资产平台资源搜索监控 GitHub，gitee

#详细点：
参考：https://www.secpulse.com/archives/124398.html
源码泄漏原因：5个方面
1、从源码本身的特性入口
2、从管理员不好的习惯入口
3、从管理员不好的配置入口
4、从管理员不好的意识入口
5、从管理员资源信息搜集入口

源码泄漏集合：
composer.json（比较经典，针对PHP）
git源码泄露（比较经典）
svn源码泄露
hg源码泄漏
网站备份压缩文件
WEB-INF/web.xml 泄露
DS_Store 文件泄露
SWP 文件泄露
CVS泄露
Bzr泄露
GitHub源码泄漏

相关利用项目：
CMS识别：https://www.yunsee.cn/
备份：敏感目录文件扫描-7kbscan-WebPathBrute
CVS：https://github.com/kost/dvcs-ripper
GIT：https://github.com/lijiejie/GitHack
SVN：https://github.com/callmefeifei/SvnHack
DS_Store：https://github.com/lijiejie/ds_store_exp

GITHUB资源搜索：
in:name test #仓库标题搜索含有关键字 
in:descripton test #仓库描述搜索含有关键字 
in:readme test #Readme文件搜素含有关键字 
stars:>3000 test #stars数量大于3000的搜索关键字 
stars:1000..3000 test #stars数量大于1000小于3000的搜索关键字 forks:>1000 test #forks数量大于1000的搜索关键字 
forks:1000..3000 test #forks数量大于1000小于3000的搜索关键字 size:>=5000 test #指定仓库大于5000k(5M)的搜索关键字 pushed:>2019-02-12 test #发布时间大于2019-02-12的搜索关键字 created:>2019-02-12 test #创建时间大于2019-02-12的搜索关键字 user:test #用户名搜素 
license:apache-2.0 test #明确仓库的 LICENSE 搜索关键字 language:java test #在java语言的代码中搜索关键字 
user:test in:name test #组合搜索,用户名test的标题含有test的
关键字配合谷歌搜索：
site:Github.com smtp 
site:Github.com smtp @qq.com 
site:Github.com smtp @126.com 
site:Github.com smtp @163.com 
site:Github.com smtp @sina.com.cn 
site:Github.com smtp password 
site:Github.com String password smt
```

## 一、cms指纹识别获取方式

网上开源的程序，得到名字就可以搜索直接获取到源码。

cms在线识别：

```plain
CMS识别：https://www.yunsee.cn/
		https://whatcms.org/
		https://searchcode.com/
		http://finger.tidesec.net/
		https://publicwww.com/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345454724-9b67e57d-6862-491a-8e11-df052ac26ee4.png)

### 案例1-cms识别-云悉指纹识别平台

```plain
cms平台，需要花钱注册，不一定需要100%:https://www.yunsee.cn/
可以直接识别cms，就可以在网上直接下载，获取到源码，根据特性来搭建识别。
识别有指纹的，像百度，淘宝这些是不可以识别的，是属于内部团队开发的。
识别到web信息，IP信息，域名信息。
如果识别不了，那么就是自己开发等源码
获取源码，获取这套源码在网上有没有出现过漏洞，这就是cms获取的意义。
没有账号，演示不了，不过也是差不多的，只是工具而已。
```

账号要花钱在线申请

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455282-822a85c5-2794-4dee-98ea-4183cf1386fb.png)

注册条件：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345454717-155e3b43-4a96-4a84-bb6e-05e5a891c81e.png)

还会查询域名等信息，不一定准确。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455112-769268c3-5aea-4d16-b563-8b1d93ca3ae7.png)

支持的识别框架

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345454850-d8be3137-bbd6-4ec2-ae43-3c80b9d24f3b.png)

## 2.习惯&配置&特性等获取方式

源码泄露原因：

```plain
源码泄漏原因：5个方面
1、从源码本身的特性入口
2、从管理员不好的习惯入口
3、从管理员不好的配置入口
4、从管理员不好的意识入口
5、从管理员资源信息搜集入口
```

### 备份文件扫描方式：

```plain
备份：敏感目录文件扫描-7kbscan-WebPathBrute
CVS：https://github.com/kost/dvcs-ripper
GIT：https://github.com/lijiejie/GitHack
SVN：https://github.com/callmefeifei/SvnHack
DS_Store：https://github.com/lijiejie/ds_store_exp
```

### 案例1-习惯不好-备份文件-某黑阔博客源码泄漏

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455871-f643069f-41aa-42b9-86d1-abd3e8da9441.png)

7kbsan扫描其扫描扫到一个1.zip，访问www.h0r2yc.com/1.zip就可以进行下载。

```plain
思路：
在站点外同目录备份，就不会出现无法下载，
如果在www.xiaodi8.com站点内的目录下下面备份不移动位置，就会出现安全问题

c:/wwwroot/www.xiaodi8.com/www.xiaodi8.com.rar 可以下载

c:/www.xiaodi8.com./rar 不能下载

就是根目录可以下载，根目录外就无法下载了
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455524-6a1adf16-7162-40fd-b06f-fe62dd17d95a.png)

这时候目录遍历漏洞有可以，访问…/时文件要能自动索引找文件时。

## 3.托管资产平台资源搜索监控 GitHub，gitee等

https://codeleading.com/article/54926468941/

```plain
源码泄漏集合：
composer.json（比较经典，针对PHP）
git源码泄露（比较经典）
svn源码泄露
hg源码泄漏
网站备份压缩文件
WEB-INF/web.xml 泄露
DS_Store 文件泄露
SWP 文件泄露
CVS泄露
Bzr泄露
GitHub源码泄漏
```

#### GITHUB资源搜索：

```plain
GITHUB资源搜索：
in:name test #仓库标题搜索含有关键字 
in:descripton test #仓库描述搜索含有关键字 
in:readme test #Readme文件搜素含有关键字 
stars:>3000 test #stars数量大于3000的搜索关键字 
stars:1000..3000 test #stars数量大于1000小于3000的搜索关键字 forks:>1000 test #forks数量大于1000的搜索关键字 
forks:1000..3000 test #forks数量大于1000小于3000的搜索关键字 size:>=5000 test #指定仓库大于5000k(5M)的搜索关键字 pushed:>2019-02-12 test #发布时间大于2019-02-12的搜索关键字 created:>2019-02-12 test #创建时间大于2019-02-12的搜索关键字 user:test #用户名搜素 
license:apache-2.0 test #明确仓库的 LICENSE 搜索关键字 language:java test #在java语言的代码中搜索关键字 
user:test in:name test #组合搜索,用户名test的标题含有test的
```

#### 谷歌搜索

```plain
关键字配合谷歌搜索：
site:Github.com smtp 
site:Github.com smtp @qq.com 
site:Github.com smtp @126.com 
site:Github.com smtp @163.com 
site:Github.com smtp @sina.com.cn 
site:Github.com smtp password 
site:Github.com String password smt
```

## 案例1-配置不当-GIT泄漏-某程序员博客源码泄漏

```plain
Git是一个开源的分布式版本控制系统，在执行git init初始化目录的时候，会在当前目录下
自动创建一个.git目录，用来记录代码的变更记录等。发布代码的时候，如果没有把.git这个目录删除，就直接发布到了服务器上，攻击者就可以通过它来恢复源代码。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455299-21f13b65-faac-4516-b28b-19b62f7beaec.png)

#### 判定存在.git文件：

访问站点/.git/

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455754-692b831f-4804-42d6-bacd-d0a62c4bbaf2.png)

#### 获取源码工具

需要python2环境运行

```plain
安装GitHack：https://github.com/lijiejie/GitHack，运行
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455661-27472bf6-697e-4cd3-9ab5-f3fd066320a3.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345455668-64504378-131d-45e4-b27b-72783082a9aa.png)

操作->打开python安装目录，在GitHack-master目录下面运行cmd

，将python文件拖进来。

执行工具语法：

```plain
C:\Python27\python.exe GitHack.py http://www.openssl.org/.git/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345456869-71c41227-ed5e-4f72-9e27-8990a50c7f9d.png)

## 案例2-配置不当-SVN泄漏-某国外小伙子源码泄漏

工具地址：https://github.com/callmefeifei/SvnHack.git

```plain
SVN是一个开放源代码的版本控制系统。在使用SVN管理本地代码过程中，会自动生成一个名为.svn的隐藏文件夹，其中包含重要的源代码信息。网站管理员在发布代码时，没有使用‘导出’功能，而是直接复制代码文件夹到WEB服务器上，这就使.svn隐藏文件夹被暴露于外网环境，可以利用.svn/entries文件，获取到服务器源码。
如git同理
trafficbonus.com/.svn/entries
用到SvnHack-master工具
python SvnHack.py -u http://x.x.x.x/.svn/entries —download

访问http://trafficbonus.com/.svn/entries，如果存在，则证明有这个漏洞：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345456053-76f92fcb-f278-4adf-bc3a-3f8e7fdfe203.png)

下载源码：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345456609-95f4dd36-b3ce-48cd-8a5f-6c687a6c2b74.png)

## 案例3-配置不当-DS_Store泄漏-某开发Mac源码泄漏

工具:https://github.com/lijiejie/ds_store_exp

```plain
.DS_Store 文件 MAC 系统是用来存储这个文件夹的显示属性的：比如文件图标的摆放位置。如果用户删除以后的副作用就是这些信息的失去。

这些文件本来是给 Finder 使用的，但它们被设想作为一种更通用的有关显示设置的元数据存储，诸如图标位置和视图设置。 当你需要把代码上传的时候，安全正确的操作应该把 .DS_Store 文件删除才正确。通过.DS_Store可以知道这个目录里所有的文件名称。

当访问/.ds_store可以访问的到，就证明存在。
```

商品网站![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345456840-1d206dca-f755-4bb1-bc65-65d4798f6ab5.png)

当访问/.ds_store可以访问的到，就证明存在。例如：

```plain
http://www.baidu.com/.ds_store
```

使用方式：

```plain
python ds_store_exp.py http://www.example.com/.DS_Store
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457111-f675516a-79c6-445b-988f-f2e12773301e.png)

此外：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345456499-9fdccfc8-4f93-4817-a328-68e07a9482dd.png)

## 案例4-PHP特性-composer.json泄漏-某直接搭建源码泄漏

PHP下

可能会存在json的配置说明文件。可能通过遗留下来的json文件获取版本信息，文件目录，cms信息等。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457081-25043183-8c9c-47b8-886c-993423b6424a.png)

## 以上这四个案例是基于获取不到cms名称情况下获取源码的方式。

## 案例五-WEB-INF泄露-RoarCTF-2019-EasyJava

这个泄露需要配合文件下载漏洞

```plain
WEB-INF是Java的WEB应用的安全目录，如果想在页面中直接访问其中的文件，必须通过web.xml文件对要访问的文件进行相应映射才能访问。

WEB-INF 主要包含一下文件或目录：
WEB-INF/web.xml : Web应用程序配置文件, 描述了servlet和其他的应用组件配置及命名规则.
WEB-INF/database.properties : 数据库配置文件
WEB-INF/classes/ : 一般用来存放Java类文件(.class)
WEB-INF/lib/ : 用来存放打包好的库(.jar)
WEB-INF/src/ : 用来放源代码(.asp和.php等)通过找到 web.xml 文件，推断 class 文件的路径，最后直接 class 文件，再通过反编译 class 文件，得到网站源码。
但是一般来说，就算知道相关路径，没有下载漏洞也不能进行测试。需要配合其他漏洞进行测试。
```

案例靶场地址：[BUUCTF在线评测 (buuoj.cn)](https://buuoj.cn/challenges)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457098-1481b7dc-8441-4c58-bbdb-1a6d6e5ee47f.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457222-7d19748d-d011-4fad-8d59-ea78fd296716.png)

### 文件下载漏洞地址：

点击help

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457293-398d6956-17d8-4a6f-bf76-4f88dcb99f79.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345458066-7045b0a6-8cf4-4af2-8fcb-8c15007ba545.png)

```plain
控制更改参数help.docx来下载WEB-INF/web.xml文件。Filename=WEB-INF/web.xml下载打开，观察到代码。
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457991-ba5c3e8d-3881-4493-80d7-6c75c18b9191.png)

## 案例六-资源监控-GITHUB泄漏-语法搜索&关键字搜索&社工

```plain
网站：blog.aabyss.cn
在GitHub中直接搜索aabyss

社工用户：Norah C.IV 一般会保留个人信息，开发者会在代码中保留，账号密码等。可以进行脚本监控，如果监控到了就会进行发送信息
1.小迪博客资源中邮箱等关键字
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345457605-4448f348-22e9-458f-a29f-f86276634c5c.png)



来自: [第7天：信息打点-资产泄漏&；CMS识别&；Git监控&；SVN&；DS_Store&；备份_云悉指纹识别平台-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/134497035?spm=1001.2014.3001.5502)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113678194-97148df1-888a-46cf-ab26-a7472ce458f4.png)

```plain
#知识点：
1.CMS指纹识别源码获取方式 源码获取方法
2.习惯&配置&特性等获取方式
3.托管资产平台资源搜索监控 GitHub，gitee

#详细点：
参考：https://www.secpulse.com/archives/124398.html
源码泄漏原因：5个方面
1、从源码本身的特性入口
2、从管理员不好的习惯入口
3、从管理员不好的配置入口
4、从管理员不好的意识入口
5、从管理员资源信息搜集入口

源码泄漏集合：
composer.json（比较经典，针对PHP）
git源码泄露（比较经典）
svn源码泄露
hg源码泄漏
网站备份压缩文件
WEB-INF/web.xml 泄露
DS_Store 文件泄露
SWP 文件泄露
CVS泄露
Bzr泄露
GitHub源码泄漏

相关利用项目：
CMS识别：https://www.yunsee.cn/
备份：敏感目录文件扫描-7kbscan-WebPathBrute
CVS：https://github.com/kost/dvcs-ripper
GIT：https://github.com/lijiejie/GitHack
SVN：https://github.com/callmefeifei/SvnHack
DS_Store：https://github.com/lijiejie/ds_store_exp

GITHUB资源搜索：
in:name test #仓库标题搜索含有关键字 
in:descripton test #仓库描述搜索含有关键字 
in:readme test #Readme文件搜素含有关键字 
stars:>3000 test #stars数量大于3000的搜索关键字 
stars:1000..3000 test #stars数量大于1000小于3000的搜索关键字 forks:>1000 test #forks数量大于1000的搜索关键字 
forks:1000..3000 test #forks数量大于1000小于3000的搜索关键字 size:>=5000 test #指定仓库大于5000k(5M)的搜索关键字 pushed:>2019-02-12 test #发布时间大于2019-02-12的搜索关键字 created:>2019-02-12 test #创建时间大于2019-02-12的搜索关键字 user:test #用户名搜素 
license:apache-2.0 test #明确仓库的 LICENSE 搜索关键字 language:java test #在java语言的代码中搜索关键字 
user:test in:name test #组合搜索,用户名test的标题含有test的
关键字配合谷歌搜索：
site:Github.com smtp 
site:Github.com smtp @qq.com 
site:Github.com smtp @126.com 
site:Github.com smtp @163.com 
site:Github.com smtp @sina.com.cn 
site:Github.com smtp password 
site:Github.com String password smt
```

- 直接获取-CMS识别-云悉指纹识别平台

```plain
cms平台，需要花钱注册，不一定需要100%:https://www.yunsee.cn/
可以直接识别cms，就可以在网上直接下载，获取到源码，根据特性来搭建识别。
识别有指纹的，像百度，淘宝这些是不可以识别的，是属于内部团队开发的。
识别到web信息，IP信息，域名信息。
如果识别不了，那么就是自己开发等源码
获取源码，获取这套源码在网上有没有出现过漏洞，这就是cms获取的意义。
没有账号，演示不了，不过也是差不多的，只是工具而已。
```

- 习惯不好-备份文件-某黑阔博客源码泄漏

```plain
http://www.h0r2yc.com/
扫描到1.zip，访问www.h0r2yc.com/1.zip就可以进行下载。
如果在www.xiaodi8.com同目录备份，就不会出现问题，无法下载到，如果在www.xiaodi8.com下面备份，就会出现安全问题
c:/wwwroot/www.xiaodi8.com/www.xiaodi8.com.rar 可以下载
c:/www.xiaodi8.com.rar 不能下载
可以下载：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113676077-3deff8a1-6e5c-4bf5-8a08-a351157e1b1f.png)

不能下载：访问域名不能下载，但是如果用IP访问的话，那么就要看IP指向的是不是www这个目录。具体能不能下载看网站情况。IP指向目录跟域名指向目录是不同的。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113676075-d7a0d006-a72f-420a-8210-7b1f981ba751.png)

利用..上一级是不可以的。没有接受进行路径操作。

- 配置不当-GIT泄漏-某程序员博客源码泄漏

```plain
Git是一个开源的分布式版本控制系统，在执行git init初始化目录的时候，会在当前目录下自动创建一个.git目录，用来记录代码的变更记录等。发布代码的时候，如果没有把.git这个目录删除，就直接发布到了服务器上，攻击者就可以通过它来恢复源代码。
把源码进行Git同步，.git目录是为了云功能同步
如果判断存在 直接在网站后面加上.git
121.36.49.234/.git,出现403发现存在目录
用到GitHub可以获取源码

判断存在：http://121.36.49.234/.git/
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113676144-81e24656-7a5d-4e91-be46-a7c5d59032ca.png)

安装GitHack：https://github.com/lijiejie/GitHack，运行

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113676984-0eabe7fd-5692-4ea9-841c-e81d7c9b4555.png)

在当前目录下生成源码文件：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677991-aec7bb8d-d177-4819-92ba-697ea8b9540c.png)

- 配置不当-SVN泄漏-某国外小伙子源码泄漏

```plain
SVN是一个开放源代码的版本控制系统。在使用SVN管理本地代码过程中，会自动生成一个名为.svn的隐藏文件夹，其中包含重要的源代码信息。网站管理员在发布代码时，没有使用‘导出’功能，而是直接复制代码文件夹到WEB服务器上，这就使.svn隐藏文件夹被暴露于外网环境，可以利用.svn/entries文件，获取到服务器源码。
如git同理
trafficbonus.com/.svn/entries
用到SvnHack-master工具
python SvnHack.py -u http://x.x.x.x/.svn/entries —download

访问http://trafficbonus.com/.svn/entries，如果存在，则证明有这个漏洞：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113676793-2fb768c6-00bd-4980-91e2-f40d79498a69.png)

执行：python2 SvnHack.py -u https://github.com/callmefeifei/SvnHack --download，但是下载失败，应该是环境的问题。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677198-014abd94-81e1-4915-a6f8-c1af908aff4e.png)

- 配置不当-DS_Store泄漏-某开发Mac源码泄漏

```plain
.DS_Store是Mac下Finder用来保存如何展示 文件/文件夹 的数据文件，每个文件夹下对应一个。如果将.DS_Store上传部署到服务器，可能造成文件目录结构泄漏，特别是备份文件、源代码文件。
zhuchao.yslts.com/.DS_Store 判断存在
工具用法：ds_store_exp.py http://hd.zj.qq.com/themes/galaxyw/.DS_Store
访问zhuchao.yslts.com/.DS_Store，直接提示下载：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677327-24545d41-4633-4b61-9de9-fc52b13d0e94.png)

下载后打开是乱码：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677598-fb05a7a8-7fb8-4de6-b557-1ed6e8d8a2c6.png)

用到工具执行python2 ds_store_exp.py http://zhuchao.yslts.com/.DS_Store 出现错误：环境问题，问题没有解决

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677559-4422a589-e42b-4bf9-abf4-28f6034b9d30.png)

- PHP特性-composer.json泄漏-某直接搭建源码泄漏

```plain
参考文章：https://blog.csdn.net/qq_35655945/article/details/79694249，可以理解为配置文件，可以通过它来获取源码文件。
配置性文件，可以通过他来获取源码信息，获取cms等
访问：english.cmdesign.com.cn/composer.json
这是PHP特性，只能在PHP源码网站才会发生
访问english.cmdesign.com.cn/composer.json：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113677842-e8e2ee6a-7f72-42f0-8dbc-7f29fcb04722.png)

- 下载配合-WEB-INF泄露-RoarCTF-2019-EasyJava

```plain
wp:https://blog.csdn.net/silencediors/article/details/102579567 需要配合下载漏洞。
网站:buuoj.cn
WEB-INF是Java的WEB应用的安全目录，如果想在页面中直接访问其中的文件，必须通过web.xml文件对要访问的文件进行相应映射才能访问。

WEB-INF 主要包含一下文件或目录：
WEB-INF/web.xml : Web应用程序配置文件, 描述了servlet和其他的应用组件配置及命名规则.
WEB-INF/database.properties : 数据库配置文件
WEB-INF/classes/ : 一般用来存放Java类文件(.class)
WEB-INF/lib/ : 用来存放打包好的库(.jar)

WEB-INF/src/ : 用来放源代码(.asp和.php等)通过找到 web.xml 文件，推断 class 文件的路径，最后直接 class 文件，再通过反编译 class 文件，得到网站源码。
但是一般来说，就算知道相关路径，没有下载漏洞也不能进行测试。需要配合其他漏洞进行测试。

打开地址，发现：http://2c49355e-4d1b-40df-899d-5d4bc42333b4.node4.buuoj.cn:81/Download?filename=help.docx，存在下载漏洞。
把filename=help.docx用post提交，就会下载这个文件：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113678288-621a0000-ba4a-4d12-9ba4-c918a4b0165a.png)

控制help.docx来下载WEB-INF/web.xml文件。Filename=WEB-INF/web.xml下载打开，观察到代码。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113678047-1edc90ff-7f3a-4cc8-bf59-2fd1855c847e.png)

构造下载地址：filename=WEB-INF/classes/com/wm/ctf/FlagController.class，下载完成后用IDE打开。

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113678361-4b0622a8-f7b8-4f09-b0f5-723c7fd898a9.png)

- 资源监控-GITHUB泄漏-语法搜索&关键字搜索&社工

```plain
网站：blog.aabyss.cn
在GitHub中直接搜索aabyss

社工用户：Norah C.IV 一般会保留个人信息，开发者会在代码中保留，账号密码等。可以进行脚本监控，如果监控到了就会进行发送信息
1.小迪博客资源中邮箱等关键字
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721113678503-a9f6991f-a91f-4d7d-af97-c3e1cc556d71.png)

```plain
也可以在bing上搜索。

2.渊龙sec安全团队blog.aabyss.cn

3.群成员 花捞的信息Norah C.IV
```





来自: [安全的小菜鸡 - 第7天：信息打点-资产泄漏&CMS识别&Git监控&SVN&DS_Store&备份](http://www.mumuxi8.com/?id=10)