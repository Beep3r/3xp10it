

### Disclaimer

```
[!] legal disclaimer: Usage of 3xp10it.py and web.py for attacking targets without prior mutual consent is 
illegal.It is the end user's responsibility to obey all applicable local, state and federal laws.Developers
assume no liability and are not responsible for any misuse or damage caused by this program.
```

### Install 

```
git clone https://github.com/3xp10it/3xp10it.git
```

### Usage

```
bash beforeWork.sh[这一步安装相关依赖,第一次使用本工具时需要运行,以后不用再运行]
python3 3xp10it.py[主程序,工作时运行,第一次运行时需要先运行上面的beforeWork.sh]
python3 web.py[可选,如果运行则要新开一个终端运行以便于查看相关输出信息,该工具为web后台]
```

### Requirement

```
need python3
need pip3
mysql
works on linux(test on ubuntu and kali2.0,others not test)

python3安装可参考如下步骤:
	apt-get install python3
	或:
	wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tar.xz
	tar xJf Python-3.5.2.tar.xz
	cd Python-3.5.2
	./configure --prefix=/opt/python3
	make && make install
	ln -s /opt/python3/bin/python3.5 /usr/local/bin/python3
	
pip3安装:
apt-get install -y python3-pip

kali linux2安装pip3可参考如下步骤:
	echo "deb-src http://http.kali.org/kali kali main non-free contrib" >> /etc/apt/sources.list
	echo "deb-src http://security.kali.org/kali-security kali/updates main contrib non-free" >> /etc/apt/sources.list
	apt-get update
	apt-get install python3-pip
```

### About

a)3xp10it是一个自动化渗透测试框架,目前没有做到完全自动化[自动上传漏洞利用框架和自动fuzz框架暂时没有加入]

b)支持功能列表

- cdn真实ip查找模块
- 旁站获取[如果在cdn模块中发现有cdn但是没有获取到真实ip则不进行旁站获取]
- 子站获取
- 高危漏洞扫描模块
- 爬虫模块
- 目标网站脚本类型检测
- 目录扫描模块
- sqli扫描模块
- robots/sitemap自动收集
- cms识别与cms漏洞扫描模块
```
cms识别支持以下cms:
    aspcms
    dedecms
    discuz
    drupal
    dvbbs
    ecshop
    emlog
    empirecms
    espcms
    foosuncms
    hdwiki
    joomla
    kesioncms
    kingcms
    ljcms
    php168
    phpcms
    phpwind
    powereasy
    qibosoft
    siteserver
    southidc
    wordpress
    z-blog
    opencart

cms漏洞扫描模块:
在cms识别基础上支持所有exploit-db.com支持的已有漏洞,不能识别的cms暂不支持查询对应exploit-db.com漏洞库数据,
wordpress,joomla,discuz这3个cms支持额外专门的对应漏洞扫描工具与基于exploit-db.com的漏洞库查询

```
- 自动识别管理员页面并爆破[支持自动识别简单验证码]
```
1.支持自动识别简单验证码
2.支持跳转到管理员登录页面直接爆破
    eg.http://127.0.0.1/wp-admin会跳转到http://127.0.0.1/wp-login.php这个真实的登录页面
    支持输入参数为http://127.0.0.1/wp-admin时直接爆破http://127.0.0.1/wp-login.php
```
- webshell自动查找与爆破,支持asp,php,aspx,jsp,支持chopper一句话webshell和大马类型webshell
```
a)apache,iis,nginx,lighttpd在phpstudy中测试默认可接收1000个post参数  
b)一句话类型webshell根据上面的特点可以正常情况下(单线程)的1000倍速度来爆破,可用大字典爆破,但是不能用多线程,
  可能是因为用多线程会太快而让web server觉得每次的参数不止1000个  
c)其他类型web server未测试,暂用多线程1倍速爆破,用最常用的webshell较小字典,17659个左右  
d)大马类型webshell由于表单名是一定的,所以不能以1000倍速爆破,于是也用和c)中一样的小字典多线程1倍速爆破  
```
- 资源文件收集
- 端口扫描模块
- 端口暴破模块
```
对21,22,1433,3306,3389端口根据开放情况使用medusa进行暴力破解
```
- whois信息收集  
- 支持人工渗透时记录笔记
```
笔记功能需要额外安装phpmyadmin等可以编辑数据库的工具,访问数据库,在targets|xxx_pang|xxx_sub表中有一个note列可用
于记录人工渗透时的笔记,查看扫描结果最好也用phpmyadmin来查看数据库的targets表和xxx_pang和xxx_sub表,如果不安装
phpmyadmin也可由web.py在web后台中查看扫描结果
```

c)可选工作模式

模式一:扫描目标和目标的所有旁站  
模式二:扫描目标和目标的所有子站  
模式三:扫描目标和目标的所有旁站和所有子站  
模式四:只扫描目标  

```
默认使用模式一扫描[在运行3xp10it后可自选],上面四种模式中的[扫描目标]里的目标支持批量导入多个目标和手工录入多个
目标.四种工作模式中与旁站和子站相关的由程序自动完成[eg.选择模式三工作时,将自动获取导入的目标的所有旁站和所有子
站,并对这些旁站和子站依次遍历上面的各个扫描模块]
```

d)特点

- 可在中断后重新运行时从断点附近接着上次的过程扫描,不用重新扫描
- 运行3xp10it后自动从数据库中取出待完成的扫描任务进行扫描
- 支持正常扫描和优先扫描两个扫描组,如果优先扫描组里有任务则先扫描优先扫描组里的目标,在添加任务时可选择将目标是
  正常扫描还是优先扫描
- 上述支持功能列表中的功能默认全部遍历扫描,如果要使用单个模块可在web界面使用
- 3xp10it配备一个web后台,web页面可查询当前扫描结果与使用单个模块功能
- 3xp10it独立于web运行,也即没有目录下的pannel文件夹也可运行
- 目录下的pannel文件夹是Django为3xp10it写的一些相关界面,web界面使用在下面介绍
- 支持找到高危漏洞邮件通知[eg.sqli,webshell爆破成功等] 

e)web后台说明

- web后台如下图,需要管理员身份登录才可进后台
<img src="https://raw.githubusercontent.com/3xp10it/pic/master/login.png">
<img src="https://raw.githubusercontent.com/3xp10it/pic/master/web.png">

- web后台相当于3xp10it的部分界面+分割的模块化工具+exp10it中没有的功能的附加工具的集合
- web后台由Django==1.10.3开发
- web后台中支持工具列表
	- targets:查看扫描目标,新增/删除扫描目标 
	- 获取旁[子]站:获取旁站或子站模块
	- xcdn:尝试识别cdn背后的真实ip
	- 高危漏扫:高危漏洞扫描模块
	- sqli:sql注入模块
	- 扫目录:目录扫描模块
	- cms漏扫:cms漏洞扫描模块
	- webshell爆破:webshell爆破模块
	- 管理员登录爆破:管理员登录爆破模块
	- waf爆破:waf自动爆破模块[3xp10it中没有这个功能]
	- dbquery:数据库语句执行接口
	- 扫描结果:查看当前扫描结果



### Detail

```
1.3xp10it需要用到bingapi,需要先申请好bingapi
2.上面的web.py不一定要运行,核心功能在3xp10it.py文件中
3.如果要后台功能需运行python3 web.py
4.如果要使用web.py,重新开机后需要重新运行web.py
5.运行web.py常见错误:端口被占用.解决方法:
a)netstat -ntlp | grep 8000
b)在a)中找到pid后kill -9 pid
c)重新运行python3 web.py
6.3xp10it中调用的是关键模块exp10it中的exp10itScanner,exp10it模块由pip3 install exp10it安装,安装路径一般如下:
/usr/local/lib/python3.5/dist-packages
7.文件分布结构如下:

当前目录
.
├── 3xp10it.py
├── pannel
│   ├── ghostdriver.log
│   ├── manage.py
│   ├── models.py[web后台没有用django的模型]
│   ├── pages[web页面的html文件,相当于django的template]
│   └── pannel[django相关文件]
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py[django配置的访问与响应规则]
│       ├── views.py[django配置的关键函数]
│       └── wsgi.py
├── readme.md
├── uninstall.py
└── web.py


/usr/local/lib/python3.5/dist-packages路径下相关文件

├── cms_identify[cms识别模块相关文件]
├── cms_scan[cms漏洞扫描模块相关文件]
├── config.ini[配置文件]
├── dicts[字典文件]
├── dirsearch[目录扫描模块相关文件]
├── exp10it.py[关键模块文件]
├── exps[exp模块相关文件]
├── log[日志文件夹]
├── tools[web后台中的各个工具]

```

### FAQ

```
Q0:config.ini 这个文件怎么没有找到?
A0:config.ini在python3 3xp10it.py初次运行后会自动生成,用于设置bing API key,发邮件的帐号,数据库连接配置,扫描模式
   等信息,一般会在/usr/local/lib/python3.5/dist-packages/config.ini这里,与python3的安装路径有关

Q1:单个模块怎么使用?
A1:单个模块执行有2种方法:
   1)web后台
   2)cd /usr/local/lib/python3.5/dist-packages/tools && ls -al

Q2:为什么需要连接google才能用?
A2:要保证能直接ping通google证明可以绕过GFW,有些domain不连vpn会无法访问,这样的domain在正常情况下被GFW拦截时会影响
   代码获取真实ip的效果,代码中强制要求连接vpn

```

### Todo

```
1.向cgc致敬,探索智能化
    refer:https://github.com/jivoi[awesome-ml-for-cybersecurity]
2.高危漏洞添加:
	heartbleed
	struts2漏洞
	ms08-067
	ms17-010	
	iis6.0 nday  http://hacktech.cn/
3.针对扫描到的开放了的未知端口尝试telnet登录测试是否是后门并针对每个漏洞进行高危模块(eg.heartbleed)检测
	https://docs.google.com/presentation/d/1-mtBSka1ktdh8RHxo2Ft0oNNlIp7WmDA2z9zzHpon8A/edit#slide=id.p67
	https://zhuanlan.zhihu.com/p/26618074
4.如果脚本是php,判断所有http请求头中是否出现反序列化字符串,如果有则报警提示人工分析看看有没有可能有漏洞
5.任意文件访问漏洞检测功能添加
6.命令执行漏洞检测功能添加
7.在扫描前支持导入cookie后再扫描,在数据库中对每个域名新加一个cookie栏
8.在爬虫的时候收集邮箱地址,之后查询社工库
	https://github.com/lauixData/leakPasswd
	http://cha.hx99.net/
9.参考如下标准完善
	https://www.processon.com/view/583e8834e4b08e31357bb727
10.爬虫时不扫描 logout 等会导致 cookie 失效的页面
```

### Changelog

```
[+] 2017-06-09 修改旁站查询接口，不再使用bing
[+] 2017-06-08 对get/post请求后对返回的数据(bytes类型)做编码判断后再根据对应编码进行解码
[+] 2017-05-29 整体扫描方案中添加delay参数防过多请求被服务器拉黑，默认delay=1s
[+] 2017-03-23 增加填写mysql服务器地址时除ip地址格式外可以填写域名格式或localhost
[+] 2017-03-23 修复网站属于http或https的判断
[+] 2017-03-23 修复子域名查找时判断根域名的逻辑
[+] 2017-03-23 修改在选择扫描模式时的默认选项为根据是否已经有config.ini文件里的扫描选项来判断
[+] 2017-03-22 修复没有检测lxml模块安装而导致的get_request函数无法成功get请求的错误
[+] 2017-02-22 增加cms漏洞扫描模块,基于exploit-db.com
[+] 2017-02-21 增加常见开放端口暴破模块
