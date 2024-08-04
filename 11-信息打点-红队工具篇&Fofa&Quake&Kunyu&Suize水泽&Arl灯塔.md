## 信息打点-红队工具篇&Fofa&Quake&Kunyu&Suize水泽&Arl灯塔

## 一、网络空间四大引擎-Fofa&Quake&Shodan&Zoomeye

```plain
对应网站：
https://fofa.so
https://quake.360.cn
https://www.shodan.io
https://www.zoomeye.org
fofa搜索结果，可以进行特定资产搜索：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345718403-6a35d404-06b6-4d90-8b62-86397b31d23f.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345717818-a65bc696-348a-48eb-80e2-3f37ce3b0845.png)

## 二、工具测评-自动化收集-

### 1、ARL灯塔

下载地址：

[GitHub - TophantTechnology/ARL: ARL(Asset Reconnaissance Lighthouse)资产侦察灯塔系统旨在快速侦察与目标关联的互联网资产，构建基础资产信息库。 协助甲方安全团队或者渗透测试人员有效侦察和检索资产，发现存在的薄弱点和攻击面。](https://github.com/TophantTechnology/ARL)

资产灯塔功能：

1. 域名资产发现和整理
2. IP/IP 段资产整理
3. 端口扫描和服务识别
4. WEB 站点指纹识别
5. 资产分组管理和搜索
6. 任务策略配置
7. 计划任务和周期任务
8. Github 关键字监控
9. 域名/IP 资产监控
10. 站点变化监控
11. 文件泄漏等风险检测
12. nuclei PoC 调用

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345717890-9a2977fa-834d-4b72-bf6f-d8008426b46f.png)

### 2、Finge-集合了fofa和360quake搜索

```plain
下载地址：https://github.com/EASY233/Finger
```

使用需要·配置fofa的api：默认线程数为30实际需要修改可以在`config/config.py`中进行修改，调用api查询功能需要从配置文件修改为自已对应的 api信息。

使用fofa搜索：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345717967-6c3fc532-1566-4e9d-95cf-3dbfc9e640d3.png)

### 3、Kunyu-集合了Shodan和Zoomey搜索

```plain
下载地址：https://github.com/knownsec/Kunyu
```

需要导入key。

使用语法：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345718103-82faab3c-e02c-4490-a531-a8bd9f3e5929.png)

结果：

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345718485-1a80a75f-27f8-4d60-aa39-743bdb68541b.png)

### 4、Suize水泽-全信息收集自动化工具

[GitHub - 0x727/ShuiZe_0x727: 信息收集自动化工具](https://github.com/0x727/ShuiZe_0x727)

在配置文件iniFile/config.ini里加入api

这里只有fofa和github的

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1720345718445-9e9da56a-e118-47f3-bb4b-88da52ed4b17.png)

## 三、工具测评-单点功能器-企查&子域名&指纹识别&社工

### 1、子域名：工具

[OneForAll 一款功能强大的子域收集工具](https://github.com/shmilylty/OneForAll)

首先下载并编辑配置文件，添加自己的`api`和个性化设置，并保留原始文件结构

添加fofa的api

```plain
config
├── api.py
├── log.py
└── setting.py
```

- [2021.06.08] - [ksubdomain 无状态子域名爆破工具](https://github.com/knownsec/ksubdomain)
- [2021.06.08] - [dnsub 一款好用且强大的子域名扫描工具](https://github.com/yunxu1/dnsub)
- [2021.06.08] - [Enumeration sub domains(枚举子域名工具)](https://github.com/FeeiCN/ESD)
- [2021.06.08] - [Layer子域名挖掘机](https://github.com/euphrat1ca/LayerDomainFinder)
- [2021.06.08] - [LangSrcCurise SRC子域名资产监控](https://github.com/LangziFun/LangSrcCurise)
- [2021.06.08] - [DiscoverSubdomain 前渗透信息探测工具集-子域名](https://github.com/coco413/DiscoverSubdomain)
- [2021.06.08] - [favihash-通过favicon.ico(网站图标)哈希来枚举子域和相关域](https://raw.githubusercontent.com/m4ll0k/Bug-Bounty-Toolz/master/favihash.py)
- [2021.06.28] - [gobusterdns 内网轻量化子域名爆破工具](https://github.com/timwhitez/gobusterdns)
- [2021.07.05] - [massdns 一款功能强大的高性能DNS子域名查询枚举工具](https://github.com/blechschmidt/massdns)

### 2、企查

- [2021.06.08] - [ENScan 基于爱企查的一款企业信息查询工具](https://github.com/wgpsec/ENScan)
- [2021.06.08] - [cDomain 利用天眼查查询企业备案](https://github.com/canc3s/cDomain)
- [2021.06.08] - [cSubsidiary 利用天眼查查询企业子公司](https://github.com/canc3s/cSubsidiary)
- [2021.06.08] - [company-crawler 天眼查爬虫&企查查爬虫，指定关键字爬取公司信息](https://github.com/bouxinLou/company-crawler)
- [2021.06.27] - [JWS-icpscan ICP备案脚本，用于快速定位资产。](https://github.com/jammny/JWS-icpscan)





来自: [第十一天：信息打点-红队工具篇&；Fofa&；Quake&；Kunyu&；Suize水泽&；Arl灯塔_红队打点 arl-CSDN博客](https://blog.csdn.net/qq_56414082/article/details/136153095?spm=1001.2014.3001.5502)



![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114124313-62c1d528-e1e3-417e-9906-fe620a4955db.png)

```plain
#知识点：
1、网络空间四大引擎-Fofa&Quake&Shodan&Zoomeye
2、自动化信息收集项目-ARL灯塔&Suize水泽&Kunyu坤舆
3、单点功能信息收集项目-企查&子域名&指纹识别&社工信息

#黑暗引擎：
https://fofa.so
https://quake.360.cn
https://www.shodan.io
https://www.zoomeye.org

#自动项目：
https://github.com/knownsec/Kunyu
https://github.com/0x727/ShuiZe_0x727
https://github.com/TophantTechnology/ARL

#相关资源：
https://forum.ywhack.com/center.php
```

- 网络空间-Fofa&Quake&Shodan&Zoomeye

```plain
https://fofa.so
https://quake.360.cn
https://www.shodan.io
https://www.zoomeye.org
fofa搜索结果，可以进行特定资产搜索：
```

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114119245-6de97a7e-8539-4feb-8ff6-3867ce63018f.png)

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114119089-db052aaf-0c4d-4eb7-bdfe-0de0ad939821.png)

- 工具测评-网络空间-四大黑暗引擎集合版项目

https://github.com/EASY233/Finger，集合了fofa和360quake搜索

- 工具测评-自动化收集-ARL灯塔&Suize水泽&Kunyu

![img](https://cdn.nlark.com/yuque/0/2024/png/1591503/1721114119039-6fa7ef3a-9d42-4439-988c-f4cf0e42d1e0.png)

- 工具测评-单点功能器-企查&子域名&指纹识别&社工

https://forum.ywhack.com/center.php



来自: [安全的小菜鸡 - 第11天：信息打点-红队工具篇&Fofa&Quake&Kunyu&Suize水泽&Arl灯塔](http://www.mumuxi8.com/?id=14)