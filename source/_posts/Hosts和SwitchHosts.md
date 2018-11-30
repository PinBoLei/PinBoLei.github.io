---
title: Hosts 和 SwitchHosts
copyright: true
date: 2018-11-29 15:17:28
tags:
- SwitchHosts
- hosts
categories:
- SwitchHosts
comments: true
---
*整理了一下关于hosts和SwitchHosts知识，希望能帮到你*

***
<!-- more -->

## 1.什么是 Hosts 和 SwitchHosts

### Hosts
`Hosts`是一个没有扩展名的系统文件，可以用记事本等工具打开，其作用就是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从Hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。

`注意`:Hosts文件配置的映射是静态的，如果网络上的计算机更改了请及时更新IP地址，否则将不能访问。

### Hosts的存储位置
`hosts`文件在不同操作系统（甚至不同Windows版本）的位置都不大一样:

**1、Windows XP / 2000 / Vista / 7 / 8 / 8.1 / 10** : `C:\windows\system32\drivers\etc\`
（XP系统无法使用bat批处理命令直接替换hosts，需手动替换后重新插拔网线或重启方使hosts生效）

**2、Windows 95 / 98 / Me**：`%WinDir%\ `（其实就是C:\WINDOWS）

**3、Linux及其他类Unix操作系统**:  `/etc/`

**4、Mac OS 9及更早的系统**:  `System Folder: Preferences`或`System folder`（文件格式可能与Windows和Linux所对应的文件不同） 

**5、Mac OS X**:  `/private/etc`（使用BSD风格的hosts文件）

**6、OS/2及eComStation**：`"bootdrive":\mptn\etc\`

**7、Android**：`/system/etc/`

**8、Symbian第1/2版手机**：`C:\system\data\`

**9、Symbian第3版手机**：`C:\private\10000882\`（能使用兼容AllFiles的文件浏览器访问。）

**10、iPhone OS**：`/etc/`(需要越狱)

**11、iPad OS**：`/private/etc`

**12、webOS**：`/etc`

### Hosts的具体作用

 **1、加快域名解析**

> 对于要经常访问的网站，我们可以通过在Hosts中配置域名和IP的映射关系，提高域名解析速度。由于有了映射关系，当我们输入域名计算机就能很快解析出IP，而不用请求网络上的DNS服务器。

**2、方便局域网用户**

> 在很多单位的局域网中，会有服务器提供给用户使用。但由于局域网中一般很少架设DNS服务器，访问这些服务器时，要输入难记的IP地址。这对不少人来说相当麻烦。可以分别给这些服务器取个容易记住的名字，然后在Hosts中建立IP映射，这样以后访问的时候，只要输入这个服务器的名字就行了。

**3、屏蔽网站（域名重定向）**

> 有很多网站不经过用户同意就将各种各样的插件安装到你的计算机中，其中有些说不定就是木马或病毒。对于这些网站我们可以利用Hosts把该网站的域名映射到错误的IP或本地计算机的IP，这样就不用访问了。在WINDOWS系统中，约定 127.0.0.1 为本地计算机的IP地址, 0.0.0.0是错误的IP地址。
> 
> 如果，我们在Hosts中，写入以下内容：
> 127.0.0.1要屏蔽的网站A的域名
> 0.0.0.0要屏蔽的网站B的域名 
> 这样，计算机解析域名A和 B时，就解析到本机IP或错误的IP，达到了屏蔽网站A 和B的目的。

**4、顺利连接系统**

> 对于Lotus的服务器和一些数据库服务器，在访问时如果直接输入IP地址那是不能访问的，只能输入服务器名才能访问。那么我们配置好Hosts文件，这样输入服务器名就能顺利连接了。

**5、虚拟域名**

> 很多时候，网站建设者需要把”软环境“搭建好，再进行上传调试。但类似于邮件服务，则需要使用域名来辅助调试，这时就可以将本地 IP
> 地址与一个”虚拟域名“做地址指向，就可以达到要求的效果，且无需花费。
> 如：127.0.0.1 网站域名之后在浏览器地址栏中输入对应的网站域名即可。

### SwitchHosts
**SwitchHosts 是一个管理、切换多个 hosts 方案的工具。它是一个免费开源软件。**
![SwitchHosts](https://img-blog.csdnimg.cn/20181108091359607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)
[下载地址:http://oldj.github.io/SwitchHosts/#cn](http://oldj.github.io/SwitchHosts/#cn)

## 2.SwitchHosts的功能
### 1）语法高亮显示
![语法高亮显示](https://img-blog.csdnimg.cn/20181108092149503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 2）允许选择多个方案
![选择多个方案](https://img-blog.csdnimg.cn/20181108092304136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 3) 点击行号可快速切换注释
![切换注释](https://img-blog.csdnimg.cn/20181108092407393.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)
