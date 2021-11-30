# web安全



kali 自动补全关闭chsh输入/bin/bash

kali 自带apache服务器,service apache2 start 默认在/var/www/html下面



-------

升级软件

apt-get update

apt-get upgrade



--------------

### 信息搜集:

- dnsenum:域名搜集工具
  - dnsenum --enum {域名}
- fierce:对子域名进行搜集
  - fierce --domin baidu.com
- snmpcheck:
  - snmpcheck -t

- dmitry:
  - dmitry wnpb {域名或IP}
- nmap:
  - -sP {IP}
  - -vv {IP}:
  - -O {IP}:收集操作系统的信息
- nping -tcp -p 445 -data {消息必须是十六进制的} {IP}

- arp:嗅探主机

  - arp-scan -l 扫描内网

- netdiscover:用自己的ip进行探测存活主机

  - netdiscover -i {网卡例如eth0}

- masscan:查看开放端口

  - -p 1-65535 {ip} --rate=100
  - --rate=100是为了防止对方开了防火墙

- nbtscan：IP和主机名的转换

  - nbtscan -hv {IP}

  - >┌──(root💀kali)-[/home/kali]
    >└─# nbtscan -hv 192.168.43.107               
    >Doing NBT name scan for addresses from 192.168.43.107
    >
    >
    >NetBIOS Name Table for Host 192.168.43.107:
    >
    >Incomplete packet, 209 bytes long.
    >
    >Name             Service          Type             
    >
    >WIN-1SJFBULDAUS  Workstation Service
    >WORKGROUP        Domain Name
    >WIN-1SJFBULDAUS  File Server Service
    >WORKGROUP        Browser Service Elections
    >WORKGROUP        Master Browser
    >__MSBROWSE__  Master Browser
    >
    >Adapter address: 00:0c:29:d8:27:37

- p0f:分析软件
  - -r /xxx/xxx/x.pcap -o 文件
- zmap:

- whatweb:用于查看网站使用的什么样的CMS


- wpscan:猜测可能使用的用户名(针对wordpress)
  - 使用这个还可以进行登录测试
  - wpscan --url 这里写要登陆的url -U 这里填写用来存放的用户名文件 -P 这里写用来存放密码的文件
- cewl:猜测可能使用的密码(针对wordpress)

### MSF框架

metaspolit

msfdb init 初始化数据库

armitage:使用图形化来操控metaspolit

- msfconsole:使用命令行来操控

  - > use exploit/multi/handler



### 密码

- crunch:生成密码字典

  - crunch 最小位数 最大位数 包含的字符 -o 输出的路径

  - > crunch 2 4 1234567890 -o /root/mima.txt

- rtgen:生成彩虹表
  - 配置文件位置一般为/usr/share/rainbowcrash/charset.txt



### CMS

- sreachsploit:漏洞库
- wafw00f:waf识别
- nikto:网站结构扫描(可以扫描网站可能有哪些文件,哪些可以访问)



### 社工

- SET:社工软件???
  - setoolkit:
    - 按照提升选择 1 2 3 2
- httrack:整站下载工具



### 服务器密码攻击

- medusa:
  - -h:目标主机
  - -u:已知用户名
  - -U:未知用户名
  - -p:已知密码
  - -P:未知密码
  - -M:协议
- hydra:
  - hydra -L [用户名字典] -P [密码字典] -t [线程] -vV -e ns [目标IP] [服务]
- wget:
  - wget -r -p -np -k www.avatrade.cn

### 打包文件破解

- rarcrack:rar

  - 例如我又一个a.rar加密文件那么我就要创建一个a.rar.xml文件

    - ><?xml version="1.0" encoding="UTF-8">
      >
      >\<rarcrack>
      >
      >​	\<abc>0123456789\</abc>这个代表范围,暴力枚举
      >
      >​	\<current>0000\</current>开始
      >
      >​	\<good_password>
      >
      >\</rarcrack>

  - rarcrack --type rar --threads 8 a.rar

- fcrackzip:

  - fcrackzip -b -c [密码类型] -l 位数 -u [文件名] -v

    - >密码类型
      >
      >​	a代表a-z
      >
      >​	1代表0-9
      >
      >​	A代表A-Z
      >
      >​	!代表特殊符号

    - -v是看到过程



### Windows常用命令

- mkdir或者md:创建文件夹
- rmdir或者rd:删除文件夹
  - /s /q 以安静的方法递归删除文件夹
- del:删除
- time:当前时间
- dir:查看目录的内容
  - dir [文件或路径]
  - /S 递归显示当前文件夹内容
- tree
- ren:{文件} {重命名}
- copy:复制文件
  - /y 文件1+文件2 文件3 :将文件1和文件2合并为文件3
- move:移动

- cls:清屏
- ver:查看当前系统的版本号
- winver:查看Windows的版本号
- whoami:查看当前的用户名称
- hostname:查看计算机名称
- vol:查看当前卷序列号
- lable:当前分区卷标
- date /t显示当前日期
- time /t显示当前时间
- start /max *.exe :以全屏方式执行程序(可以不是exe)

- tasklist

- exit 0:关闭命令行

- systeminfo:查看当前系统的信息

- wmic:查看硬件

  - wmic logicdisk:查看电脑上所有的逻辑磁盘

- format :格式化

  - > format d:/FS:ntfs /Q
    >
    > 快速格式化d盘为ntfs格式

- chkdsk /f d:

- netsh advfirewall set allprofiles state off:关闭防火墙

- ![image-20211108103202182](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211108103202182.png)

- find:从文件中查找相关内容

  - /N:带有行号显示结果
  - /I:忽略大小写
  - /C:出现次数



#### 命令执行顺序

- & :按顺序执行命令无论命令是否成功

- &&:按顺序执行命令但是一旦出错后面的命令不会执行

- ||:按顺序执行多条命令一旦有一个命令正确就不会继续执行后面的程序了

- | :管道命令

- \> :覆盖重定向

- \>\>:追加重定向

- < :从文件中获取信息(输入)

- @ :命令修饰符

- , :代替空格

  - > cd .. == cd,..

- ; :执行相同命令但是参数不同

  - > ```shell
    > 
    > C:\Users\lll>dir d:\home;d:\diary
    >  驱动器 D 中的卷没有标签。
    >  卷的序列号是 08C0-CE3A
    > 
    >  d:\home 的目录
    > 
    > 2021/08/10  浩下午 07:49    <DIR>          .
    > 2021/08/10  浩下午 07:49    <DIR>          ..
    > 2021/08/10  浩下午 07:49    <DIR>          ruoyi
    >                0 个文件              0 字节
    > 
    >  d:\diary 的目录
    > 
    > 2021/11/08  于上午 09:56    <DIR>          .
    > 2021/11/08  于上午 09:56    <DIR>          ..
    > 2019/07/27  浩下午 09:11             4,484 #Diray day third
    > 2019/12/05  浩下午 05:27            12,074 Android开发
    > 2021/03/07  浩下午 02:11             3,124 Bootstrap.md
    > 2019/07/25  于上午 10:35             4,628 Diary day first.YH
    > 2019/07/31  浩下午 07:33               168 Diray day fifth.YH
    > 2019/07/28  浩下午 01:39             1,411 Diray day fourth.YH
    > 2019/07/26  浩下午 02:24             2,620 Diray day second.YH
    > 2020/10/26  浩下午 08:28            19,337 Django.md
    > 2021/10/01  浩下午 03:28    <DIR>          gitDiary
    > 2020/09/17  于上午 07:44             1,617 Git使用手册.md
    > 2020/09/25  浩下午 03:02             2,881 hexo.md
    > 2021/04/25  浩下午 08:12             3,505 HTML学习.md
    > 2021/04/12  浩下午 04:02             1,225 JavaScript.md
    > 2021/03/03  浩下午 03:27            14,004 JavaWeb.md
    > 2021/11/03  于上午 10:23            18,682 Java面试.md
    > 2020/11/04  浩下午 03:54                 0 JUC.md
    > 2019/11/01  于上午 11:22             2,879 linux
    > 2021/09/09  浩下午 04:22            22,494 Linux.md
    > 2020/08/17  浩下午 01:09             5,640 MyBatis-Plus.md
    > 2021/01/26  浩下午 04:04             8,909 Mybatis.md
    > 2021/01/16  浩下午 09:46               500 mybatis中遇到没想通的问题.md
    > 2020/12/03  浩下午 06:48             3,716 MySQL.md
    > 2021/10/19  浩下午 05:02             4,001 Nginx.md
    > 2019/12/12  浩下午 09:07            28,284 Room.emmx
    > 2019/07/28  于上午 10:26             3,151 scrapy.YH
    > 2019/07/27  于上午 10:45               673 splash
    > 2020/09/20  浩下午 05:04               153 Splash.md
    > 2021/09/17  于上午 11:36            23,554 Spring.md
    > 2021/09/14  浩下午 03:26            26,974 SpringBoot-V2.md
    > 2021/09/26  浩下午 04:54            51,890 springBoot.md
    > 2021/03/04  于上午 09:32             6,441 SpringMVC.md
    > 2021/10/01  浩下午 03:16             4,460 SpringSecurity.md
    > 2021/04/07  浩下午 07:45             4,074 SSM整合.md
    > 2021/07/23  浩下午 02:45             6,024 Swagger.md
    > 2019/12/27  于上午 08:15            26,519 Swing.emmx
    > 2019/09/19  于上午 09:30               392 test_diary_firstday.rdy
    > 2019/12/28  浩下午 11:34            31,711 textbook_knowledge.emmx
    > 2020/09/17  于上午 06:57    <DIR>          Typora
    > 2021/08/31  浩下午 04:54            32,922 Vue.md
    > 2021/11/08  于上午 09:56             4,826 web安全.md
    > 2019/11/17  浩下午 02:37    <DIR>          wolqaqldiu@163.com
    > 2019/10/24  浩下午 08:22             3,026 word_and_example
    > 2019/11/20  浩下午 10:25               397 word_diray
    > 2020/07/24  浩下午 08:38             2,581 xml标签.md
    > 2020/09/05  浩下午 05:27           322,849 XPath语法.png
    > 2019/12/17  浩下午 08:47           135,890 中国近代史.emmx
    > 2020/05/20  于上午 11:37             6,032 处理机调度的层次和调度算法的目标.emmx
    > 2019/12/27  浩下午 03:31             9,293 捕鱼达人程序.emmx
    > 2021/10/09  浩下午 03:52             4,106 整理Maven依赖.md
    > 2020/09/13  浩下午 07:58             3,363 正则和XPath.md
    > 2021/04/26  于上午 11:49             2,312 深入浅出MySQL.md
    > 2020/08/24  浩下午 05:57             5,223 网络编程.md
    > 2021/06/22  于上午 08:43            41,264 虫术.md
    > 2020/11/08  浩下午 04:20            16,352 进程学习.md
    > 2021/03/09  于上午 08:07               220 金融网站搭建.md
    >               52 个文件        942,855 字节
    >                5 个目录 254,010,781,696 可用字节
    > ```



### 注册表

regedit

> reg add 添加一条指令
>
> reg del 删除一条指令
>
> reg export 导出某个注册表文件
>
> reg import 导入某个注册表文件
>
> req query 查询





### 请求头

springsecurity默认的请求头包含：(前三个是缓存有关)

```yaml
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Content-Type-Options: nosniff
Strict-Transport-Security: max-age=31536000 ; includeSubDomains
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=b
```

Cache-Control:http1.1中引入的缓存字段

第一个no-cache代表缓存需要认证如果用户需要数据就算有缓存也要发送认证请求,而服务器回去判断是否过期如果没有过期则返回304.第二个代表不进行缓存.第三个代表缓存的有效期,这个有效期并非时间戳而是一个秒数.第四个表示缓存在使用一个陈旧的资源时必须先认证,过期则不再进行使用

Pragma:

和no-cache作用差不多,但是不能完全代替,作用是用来进行兼容http1.0的

Expires:

指定一个日期在指定的日期过期如果为0则代表过期



#### Range

请求实体内容,请求部分的内容

bytes=0~499,前500个字节

bytes=-500,最后500个字节

bytes=500-,500以及以后的所有字节



x-forward-for,即XXF头,代表请求端的IP



##### X-Content-Type-Options

这个代表禁止MIME嗅探,因为以前不会严格检测Content-Type如果缺失或者类型有问题客户端回自动检测,这就可能触发xss攻击

而这个标志就是让客户端直接遵循Content-Type不进行认证

如果想要关闭只需要

.headers().contentTypeOptions().disable()



##### Strict-Transport-Security

用来指定客户端只能通过HTTPS来访问不能通过HTTP来访问

Strict-Transport-Security: max-age=31536000 ; includeSubDomains

max-age:在多少秒之内只能使用HTTPS来访问

includeSubDomains:这个参数是可选的,表示第一条指令也适用于这个域名



##### X-Frame-Options

代表是否能够允许在页面中使用<frame>,<iframe>,<embed>,<object>中展示通过请求头保证网站没有嵌入其他网站的节点防止被劫持攻击

X-Frame-Options的取值:

- deny:不允许frame嵌套,就算相同域名也不可以,默认
- sameorigin:相同域名可以在frame中展示
- allow-from url:可以获取指定url的frame



##### X-XSS-Protection

当遇到XSS攻击时停止浏览器的加载一共有四种不同的取值方式:

0代表禁止XSS过滤

1代表启用XSS(浏览器默认的),如果检测到跨站攻击则清除页面缓存

1;mode=block遇到XSS攻击浏览器则不会清除而是停止加载

1;report=<reporting-URI>表示启用XSS过滤。如果检测到跨站脚本攻击，浏览器将清除页面，并使用CSP report-uri指令的功能发送违规报告（Chrome支持）。



##### Content-Security-Policy

内容安全策略，用于检测和削弱特定的攻击

参数一共有以下几种：

- default-src 'self'：默认情况下所有资源只能从当前域中加载。接下来细化的配置会覆盖default-src，没有细化的选项则使用default-src。
- script-src 'self'：表示脚本文件只能从当前域名加载。
- object-src 'none'：表示object标签不加载任何资源。
- style-src cdn.javaboy.org：表示只加载来自cdn.javaboy.org的样式表。
- img-src *：表示可以从任意地址加载图片。
- child-src https：表示必须使用HTTPS来加载frame。

想要了解更多可以查看https://www.w3.org/TR/CSP2/



#### 响应头

server:服务器名称

Last-Modified:资源的最后修改时间

Location:告诉浏览器去访问哪个页面

Refresh:告诉浏览器定时刷新



### 注入漏洞

#### MySql注入



1.版本测试

select id/*!55555,username*/ from users

如果版本高于5.55.55则会被执行

2.使用元数据查找数据库的信息

https://www.cnblogs.com/Xjng/p/7136424.html



3.查看注入参数个数

使用union,他是用来链接两个或多个查询语句来组成一个结果集,每列的类型必须相同

例如select id,username,password from users union select 1,2,3

这条语句再MySQL,SQL Server,Oracle中各种错误信息

MySQL:正常

SQL Server:语句错误,数据类型不匹配,无法正常执行

Oracle:语句错误,数据类型不匹配,无法正常执行

所以最好将1,2,3换成NULL,NULL,NULL来测试



4.利用函数

可以使用load_file()来获得系统的文件内容

例如

```sql
select load_file("/etc/passwd");
select load_file(0x2F6563742F706173737764)#为/etc/passwd的十六进制
select load_file(char(47,101,99,116,47,112,97,115,115,119,100))#为上面的ASCII
select hex(load_file(char(47,101,99,116,47,112,97,115,115,119,100)))#可以使用hex将函数转化为十六进制
```

这些操作需要mysql对应的用户具有足够的权限



into outfile写入文件

例如

> select 111 into outfile "文件路径"

| 函数                 | 效果                                       |
| -------------------- | ------------------------------------------ |
| length()             | 返回字符串长度                             |
| substring()          | 截断字符串                                 |
| ascii()              | 返回ASCII码                                |
| hex()                | 转化为16进制                               |
| now()                | 返回当前时间                               |
| unhex()              | 反hex                                      |
| floor()              | 返回不大于x打正整数                        |
| md5()                | 使用md5加密                                |
| group_concat()       | 返回一个来自组的链接的非NULL值的字符串结果 |
| @@datadir            | 获取数据库路径                             |
| @@basedir            | 获取mysql安装路径                          |
| @@version_compile_os | 系统版本                                   |
| user()               | 用户名                                     |
| current_user()       | 当前用户名                                 |
| system_user()        | 系统用户名                                 |
| database()           | 数据库名称                                 |
| version()            | 数据库版本                                 |



5.宽字节注入

https://blog.csdn.net/qq_35191331/article/details/77206519

宽字符:
（1）%27 '
（2）%20 空格
（3）%23 #
（4）%df 運（说明,也可以是其他的%d1之类，解析之后变成中文字符）



6.长字符截断

> 例如我有一张表是
>
> create table users{
>
> ​	id int(11) not null,
>
> ​	username varchar(7) not null,
>
> ​	password varchar(12) not null
>
> }
>
> 这样用户名最大长度为7位当我们插入了8位的时候MySQL会自动帮我们截断并且默认是warning
>
> 例如数据库中有一个管理员用户是admin
>
> 我们使用这条语句可以模仿插入
>
> insert into users(id,username,password) values(3,"admin   x","admin");
>
> 这样数据库就会将x截断不保留
>
> 我们就可以使用自己的admin进行认证   

7.使用延迟注入来判断是否注入成功

> sleep(n)
>
> 可以使用这个函数来判断
>
> 如果网站没有回显可以使用这个来判断是否执行成功
>
> and if(length(user())=0,sleep(3),1) 使用,来进行依次运算如果第一个成功则会延迟三秒
>
> and if(hex(mid(user(),1,1))=1,sleep(3),1)取出第一个字符,然后与ascii码进行循环对比
>
> and if(hex(mid(user(),L,1))=N,sleep(3),1)这样一个一个破解来判断用户名
>
> 这样可以用于无回显的网站77



#### SQL注入防范

- 数字类型注入

Integer.parseInt(request.getParameter("id"));

对于Java和C#这种语言可以使用上面这种方式来防止SQL注入,因为强制转换就会报错(几乎屏蔽所有的数字型注入)

而对于PHP和ASP这种会自动检测为String不能强制转换



- 二次注入

有些为了防止发生SQL注入就会将特殊字符进行转义,例如:

insert into users values("111'","111","111")

实际执行的为:

insert into users values("111\\'","111","111")

虽然可以屏蔽第一次的SQL注入但是数据库中的数据为

111' 111 111

这样在下次查询的时候就会造成SQL注入因为存在'



#### 注入练习

##### less1

1.首先猜测闭合使用\结果为

![image-20211125150138133](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125150138133.png)

可以看出闭合为'

2.然后测试字段数量用order by来从1开始测试

![image-20211125150403164](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125150403164.png)

为4的时候报错所以猜测sql语句为

select \*,\*,\* from \* where id = ($id) limit 0,1 

3.使用union select来测试查看哪些字段回显(前面的select要让他找不到才会回显union后面的)

![image-20211125150751151](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125150751151.png)

![image-20211125150757060](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125150757060.png)

通过结果来看回显2,3字段

4.查找库和库中的表

![image-20211125150853754](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125150853754.png)

查找库使用select database();

![image-20211125151414102](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125151414102.png)

获取库中所有的表名称

![image-20211125151509737](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125151509737.png)

4.查询所有的列并曝光库

![image-20211125151702448](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125151702448.png)

![image-20211125151709302](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211125151709302.png)

查到字段后进行曝光数据库



##### less5

先找到闭合然后利用下面的报错注入进行爆破数据库

union select extractvalue(1,concat(0x7e,(select database()),0x7e))  --+





### Set-Cookie

https://blog.csdn.net/u012717614/article/details/79131997



### HTTPOnly

只要使用了这个js就无法获取该Cookie了



### 注册表

Windows的注册表被放在system.dat和user.dat下面可以使用regedit和.reg来编辑注册表

[https://zh.wikipedia.org/zh-cn/%E6%B3%A8%E5%86%8C%E8%A1%A8#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B](https://zh.wikipedia.org/zh-cn/注册表#数据类型)

#### 注册表的数据类型:

| **显示类型（在编辑器中）** | 中文名       | 说明                                                         |
| -------------------------- | ------------ | ------------------------------------------------------------ |
| REG_SZ                     | 字符串       | 文本字符串                                                   |
| REG_BINARY                 | 二进制       | 不定长的二进制字符串,以十六进制显示                          |
| REG_DWORD                  | 双数         | 一个 32 位的二进制值，显示为 8 位的十六进制值                |
| REG_MULTI_SZ               | 多字符串     | 含有多个文本值的字符串，此名来源于字符串间用 nul 分隔、结尾两个 nul |
| REG_EXPAND_SZ              | 可扩展字符串 | 含有环境变量的字符串                                         |



#### 表结构

一般的:

|        名称         |                           作用                            |
| :-----------------: | :-------------------------------------------------------: |
|  HKEY_CLASSES_ROOT  | 存储Windows可识别的文件类型的详细列表，以及相关联的程序。 |
|  HKEY_CURRENT_USER  |                 存储当前用户设置的信息。                  |
| HKEY_LOCAL_MACHINE  |          包括安装在计算机上的硬件和软件的信息。           |
|     HKEY_USERS      |               包含使用计算机的用户的信息。                |
| HKEY_CURRENT_CONFIG |          这个分支包含计算机当前的硬件配置信息。           |



WindowsNT系列的:

|    名称    |         注册表分支          |                             作用                             |
| :--------: | :-------------------------: | :----------------------------------------------------------: |
|   SYSTEM   |  HKEY_LOCAL_MACHINE\SYSTEM  |                  存储计算机硬件和系统的信息                  |
| NTUSER.DAT |      HKEY_CURRENT_USER      | 存储用户参数选择的信息（此文件放置于用户个人目录，和其他注册表文件是分开的） |
|    SAM     |   HKEY_LOCAL_MACHINE\SAM    |                      用户及密码的数据库                      |
|  SECURITY  | HKEY_LOCAL_MACHINE\SECURITY |                        安全性设置信息                        |
|  SOFTWARE  | HKEY_LOCAL_MACHINE\SOFTWARE |                        安装的软件信息                        |
|  DEFAULT   |     HKEY_USERS\DEFAULT      |                      缺省启动用户的信息                      |
|  USERDIFF  |         HKEY_USERS          |                  管理员对用户强行进行的设置                  |



- HKEY_LOCAL_MACHINE(核心system.dat)

- HKEY_USERS(核心user.dat)

- HKEY_CURRENT_USER

  - 从/HKEY_USERS的登录用户名下获取的

- HKEY_CLASSES_ROOT:(HKEY_LOCAL_MACHINE/Software/Classes的快捷方式)

  - 记录的是系统中各类文件与其应用程序之间的关联关系

  - ActiveX类的储存等内容

  - HKEY_CLASSES_ROOT主键下的子键很简单，主要包括两类，一类是文件扩展名子键，另一类是文件类型子键。文件扩展名子键主要包括系统内定的文件扩展名和应用程序自储存的扩展名，文件扩展名子键均以“.”开头，后跟文件扩展名，可以包括任意多个字符；“*”子键和其他的不以“.”开头的子键是类储存子键，其中包括文件类型、类标识符以及程序标识符。文件名扩展子键中指明了该类文件的关联文件类型以及打开方式等。

  - >HKEY_CLASSES_ROOT主键中的文件类型子键下的常见子键的含义：
    >Defaulticon：默认的该类文件的显示图标，即我们在文件夹中看到的图标。
    >Shell：程序外壳子键
    >Shell/open/command：打开该类文件的外壳程序，默认值为相应程序的路径、名称及其参数
    >Shell/edit/command：编辑该类文件的外壳程序，默认值为相应程序的路径、名称及其参数
    >Shell/print/command：打印该类文件的外壳程序，默认值为相应程序的路径、名称及其参数
    >HKEY_CLASSES_ROOT主键下还有一个重要的子键“CLSID”，该子键下记录了所有的已注册的系统类标识符。

- HKEY_CURRENT_CONFIG

  

命令行工具reg:

>Windows Registry Editor Version 5.00
>
>[HKEY_CLASSES_ROOT\wodiu]
>@="Demo2Protocol"
>"URL Protocol"="D:\\software\\Xshell.exe"
>
>[HKEY_CLASSES_ROOT\wodiu\DefaultIcon]
>@="D:\\software\\Xshell.exe,1"
>
>[HKEY_CLASSES_ROOT\wodiu\shell]
>
>[HKEY_CLASSES_ROOT\wodiu\shell\open]
>
>[HKEY_CLASSES_ROOT\wodiu\shell\open\command]
>@="\"D:\\software\\Xshell.exe\" \"%1\""

使用上面这个.reg文件就可以在注册表中注册wodiu这个命令可以在浏览器中直接执行wodiu://来打开Xshell

@=代表赋予默认值,(这个文件中不能存在中文否则乱码)

最后一条代表这个命令与那个程序相关联



但是......为啥Http就可以不用谈而这个就要弹出



### FreeBuf

https://www.freebuf.com/articles/network/276159.html



### telnet发送邮件

https://blog.csdn.net/weixin_43605701/article/details/90204704



### 反编译apk到java

https://www.cnblogs.com/yz820/p/9466763.html



### WAF绕过

#### 1.可以使用pipline来绕过

使用BurpSuite去掉勾选Content-Length更新并且我们在请求的时候可以发现默认请求数据包时Connection为close，修改包的时候将其修改为keep-alive，Content-Length值为4

并且在请求头后面紧接着加上另一个请求头

![image-20211123174959771](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211123174959771.png)

发送一个类似这种的包含两个请求，这样有的waf在防护的时候只检测第一个，发现第一个请求合法则直接放行而第二个不会进行检测



#### 2.利用分块

由于HTTP1.1提供分块特性所以可以使用分块语句来越过waf的检查，在请求头中设置Transfer-Encoding:chunked这样就可以进行分块传递了

![image-20211123180206388](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211123180206388.png)

可以看出分成四段最后一段一定是06

### xray

#### github地址

https://github.com/chaitin/xray

下载解吖后可以直接运行（注意要用powershell如果是Windows的话）



#### 使用代理及进行扫描



首先安装ca证书

运行

> .\xray_windows_amd64.exe genca

如果是Windows双击ca.crt然后按照下面这个步骤走

![image-20211129144200596](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211129144200596.png)



如果是Linux生成证书后运行下面两个命令

```
sudo cp ca.crt /usr/local/share/ca-certificates/xray.crt
sudo update-ca-certificates
```

便配置完成



然后在配置文件中修改如下图为自己的代理(我的是本地的7890)

![image-20211129144431842](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211129144431842.png)

==这样就可以让xray走自己本地开启的代理==,比如翻个墙



如果是想要让xray监听自己的浏览器动向然后进行扫描

使用这个命令来让xray监听7777端口

> .\xray_windows_amd64.exe webscan --listen 127.0.0.1:7777 --html-output xray-testphp.html



然后将本机代理修改为7777

![img](https://docs.xray.cool/assets/tutorial/ie_configure_proxy_3.jpg)

接下来浏览器进入什么网站便开始扫描什么网站

#### 常用的命令

windows

> ./xray_windows_amd64 webscan --basic-crawler http://testphp.vulnweb.com/ --html-output xray-crawler-testphp.html

Linux

> ./xray_linux_amd64 webscan --basic-crawler http://testphp.vulnweb.com/ --html-output xray-crawler-testphp.html

--basic-crawler:要爬取的基本网站

--html-output :后接要输出的文件名称(就是将扫描结果输出到指定的文件中)



#### 配置文件

```yaml
parallel: 30                      # 漏洞探测的 worker 数量，可以简单理解为同时有 30 个 POC 在运行
http:
  proxy: ""                             # 漏洞扫描时使用的代理，如: http://127.0.0.1:8080。 如需设置多个代理，请使用 proxy_rule 或自行创建上层代理
  proxy_rule: []                        # 漏洞扫描使用多个代理的配置规则, 具体请参照文档
  dial_timeout: 5                       # 建立 tcp 连接的超时时间
  read_timeout: 10                      # 读取 http 响应的超时时间，不可太小，否则会影响到 sql 时间盲注的判断
  max_conns_per_host: 50                # 同一 host 最大允许的连接数，可以根据目标主机性能适当增大
  enable_http2: false                   # 是否启用 http2, 开启可以提升部分网站的速度，但目前不稳定有崩溃的风险
  fail_retries: 0                       # 请求失败的重试次数，0 则不重试
  max_redirect: 5                       # 单个请求最大允许的跳转数
  max_resp_body_size: 2097152           # 最大允许的响应大小, 默认 2M
  max_qps: 500                          # 每秒最大请求数
  allow_methods:                        # 允许的请求方法
  - HEAD
  - GET
  - POST
  - PUT
  - PATCH
  - DELETE
  - OPTIONS
  - CONNECT
  - TRACE
  - MOVE
  - PROPFIND
  headers:
    User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
    # Cookie: key=value

```

##### http.proxy

```yaml
http://127.0.0.1:1111
https://127.0.0.1:1111
socks5://127.0.0.1:1080
```

如果代理需要认证，可以使用下面的格式 `http://user:password@127.0.0.1:1111`

##### http.porxy_rule

```yaml
proxy_rule:
  - match: "*host1"
    servers:
      - addr: "http://127.0.0.1:8001"
        weight: 1
      - addr: "http://127.0.0.1:8002"
        weight: 2
  - match: "*"
    servers:
      - addr: "http://127.0.0.1:8003"
        weight: 1
      - addr: "http://127.0.0.1:8004"
        weight: 5
```

根据不同的域名设置代理

weight代表权重

##### http.max_qps

代表发包速度



##### plugins.xss

```yaml
enabled: true
detect_xss_in_cookie: true          # 是否探测入口点在 cookie 中的 xss
ie_feature: false                   # 是否扫描仅能在 ie 下利用的 xss要包括 expression xss、hidden tag xss、utf-7 和 content type sniffing 导致的 xss不懂最好不要开
```



##### plugins.dirscan

```yaml
dirscan:
	enabled: true
	depth: 1                            # 检测深度，定义 http://t.com/a/ 深度为 1, http://t.com/a 深度为 0
	dictionary: ""                      # 自定义检测字典, 配置后将与内置字典**合并**
```



##### plugins.sqldet

```yaml
sqldet:
    enabled: true
    boolean_based_detection: true       # 是否检测布尔盲注
    error_based_detection: true         # 是否检测报错注入
    time_based_detection: true          # 是否检测时间盲注
    use_comment_in_payload: false       # 在 payload 中使用 or, 慎用！可能导致删库！
    detect_sqli_in_cookie: true         # 是否检查在 cookie 中的注入
    dangerously_use_comment_in_sql: false #允许检查注入的时候使用注释.....开启之后可以增加检测率，但是有破坏数据库数据的可能性，请务必了解工作原理之后再开启
```



##### plugins.phantasm

```yaml
phantasm:              # poc 插件
  enabled: true
  depth: 1
  auto_load_poc: false # 除内置 poc 外，额外自动加载当前目录以 "poc-" 为文件名前缀的POC文件，等同于在 include_poc 中增加 "./poc-*"
  exclude_poc: []    # 排除哪些 poc, 支持 glob 语法, 如: "/home/poc/*thinkphp*" 或 "poc-yaml-weblogic*"
  include_poc: []    # 只使用哪些内置 poc 以及 额外加载哪些本地 poc, 支持 glob 语法, 如："*weblogic*" 或 "/home/poc/*"
                       # 也可使用 --poc 仅运行 指定的内置或本地 poc，进行测试。
                       # 例如，可使用如下命令，仅运行当前目录下的 poc 且 不运行内置 poc 进行测试：
                       # webscan -poc ./poc-* -url http://example.com
```

![image-20211129161426336](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211129161426336.png)

#### 被动代理配置

```yaml
mitm:
  ca_cert: ./ca.crt                     # CA 根证书路径
  ca_key: ./ca.key                      # CA 私钥路径
  basic_auth:                           # 基础认证的用户名密码
    username: ""
    password: ""
  allow_ip_range: []                    # 允许的 ip，可以是 ip 或者 cidr 字符串
  restriction:                          # 代理能够访问的资源限制, 以下各项为空表示不限制
    hostname_allowed: []                # 允许访问的 Hostname，支持格式如 t.com、*.t.com、1.1.1.1、1.1.1.1/24、1.1-4.1.1-8
    hostname_disallowed:                # 不允许访问的 Hostname，支持格式如 t.com、*.t.com、1.1.1.1、1.1.1.1/24、1.1-4.1.1-8
    - '*google*'
    - '*github*'
    - '*.gov.cn'
    - '*chaitin*'
    - '*.xray.cool'
    port_allowed: []                    # 允许访问的端口, 支持的格式如: 80、80-85
    port_disallowed: []                 # 不允许访问的端口, 支持的格式如: 80、80-85
    path_allowed: []                    # 允许访问的路径，支持的格式如: test、*test*
    path_disallowed: []                 # 不允许访问的路径, 支持的格式如: test、*test*
    query_key_allowed: []               # 允许访问的 Query Key，支持的格式如: test、*test*
    query_key_disallowed: []            # 不允许访问的 Query Key, 支持的格式如: test、*test*
    fragment_allowed: []                # 允许访问的 Fragment, 支持的格式如: test、*test*
    fragment_disallowed: []             # 不允许访问的 Fragment, 支持的格式如: test、*test*
    post_key_allowed: []                # 允许访问的 Post Body 中的参数, 支持的格式如: test、*test*
    post_key_disallowed: []             # 不允许访问的 Post Body 中的参数, 支持的格式如: test、*test*
  queue:
    max_length: 3000                    # 队列长度限制, 也可以理解为最大允许多少等待扫描的请求, 请根据内存大小自行调整
  proxy_header:
    via: ""                             # 是否为代理自动添加 Via 头
    x_forwarded: false                  # 是否为代理自动添加 X-Forwarded-{For,Host,Proto,Url} 四个 http 头
  upstream_proxy: ""                    # 为 mitm 本身配置独立的代理

```

##### mitm.auth

xray 支持给代理配置基础认证的密码，当设置好 `auth` 中的 `username` 和 `password` 后，使用代理时浏览器会弹框要求输出用户名密码，输入成功后代理才可正常使用。



##### mitm.queue

防止请求的网页请求资源过多设立的每十秒检测以下请求队列长度如果大于则新的请求会被不响应



##### mitm.upstream_proxy

![image-20211129162901428](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20211129162901428.png)

这样可以使用8080来记录一些信息,防止大量的攻击请求也经过8080



#### 基础爬虫配置

```yaml
basic-crawler:
  max_depth: 0                          # 最大爬取深度， 0 为无限制
  max_count_of_links: 0                 # 本次爬取收集的最大链接数, 0 为无限制
  allow_visit_parent_path: false        # 是否允许爬取父目录, 如果扫描目标为 t.com/a/且该项为 false, 那么就不会爬取 t.com/ 这级的内容
  restriction:                          # 爬虫的允许爬取的资源限制, 为空表示不限制。爬虫会自动添加扫描目标到 Hostname_allowed。
    hostname_allowed: []                # 允许访问的 Hostname，支持格式如 t.com、*.t.com、1.1.1.1、1.1.1.1/24、1.1-4.1.1-8
    hostname_disallowed:                # 不允许访问的 Hostname，支持格式如 t.com、*.t.com、1.1.1.1、1.1.1.1/24、1.1-4.1.1-8
    - '*google*'
    - '*github*'
    - '*.gov.cn'
    - '*chaitin*'
    - '*.xray.cool'
    port_allowed: []                    # 允许访问的端口, 支持的格式如: 80、80-85
    port_disallowed: []                 # 不允许访问的端口, 支持的格式如: 80、80-85
    path_allowed: []                    # 允许访问的路径，支持的格式如: test、*test*
    path_disallowed: []                 # 不允许访问的路径, 支持的格式如: test、*test*
    query_key_allowed: []               # 允许访问的 Query Key，支持的格式如: test、*test*
    query_key_disallowed: []            # 不允许访问的 Query Key, 支持的格式如: test、*test*
    fragment_allowed: []                # 允许访问的 Fragment, 支持的格式如: test、*test*
    fragment_disallowed: []             # 不允许访问的 Fragment, 支持的格式如: test、*test*
    post_key_allowed: []                # 允许访问的 Post Body 中的参数, 支持的格式如: test、*test*
    post_key_disallowed: []             # 不允许访问的 Post Body 中的参数, 支持的格式如: test、*test*
  basic_auth:                           # 基础认证信息
    username: ""
    password: ""
```

##### 子域名

```yaml
subdomain:
  max_parallel: 30                      # 子域名探测的并发度
  allow_recursion: false                # 是否允许递归探测, 开启后，扫描完一级域名后，会自动将一级的每个域名作为新的目标
  max_recursion_depth: 3                # 最大允许的递归深度, 3 表示 3 级子域名 仅当 allow_recursion 开启时才有意义
  web_only: false                       # 结果中仅显示有 web 应用的, 没有 web 应用的将被丢弃
  ip_only: false                        # 结果中仅展示解析出 IP 的，没有解析成功的将被丢弃
  servers:                              # 子域名扫描过程中使用的 DNS Server
  - 8.8.8.8
  - 8.8.4.4
  - 223.5.5.5
  - 223.6.6.6
  - 114.114.114.114
  sources:
    brute:
      enabled: true
      main_dict: ""                     # 一级大字典路径，为空将使用内置的 TOP 30000 字典
      sub_dict: ""                      # 其他级小字典路径，为空将使用内置过的 TOP 100 字典
    httpfinder:
      enabled: true                     # 使用 http 的一些方式来抓取子域名，包括 js, 配置文件，http header 等等
    dnsfinder:
      enabled: true                     # 使用 dns 的一些错误配置来找寻子域名，如区域传送（zone transfer)
    certspotter:                        # 下面的通过 api 获取的了
      enabled: true
    crt:
      enabled: true
    hackertarget:
      enabled: true
    qianxun:
      enabled: true
    rapiddns:
      enabled: true
    sublist3r:
      enabled: true
    threatminer:
      enabled: true
    virusTotal:
      enabled: true
      

```

