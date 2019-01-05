---
title: python常用模块
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python模块
---

## ipython 交互式python shell
[ipython一些操作](https://www.tuicool.com/articles/aqIrYjv)

## Autopy
~~~
Autopy是一个简单的跨平台的Python图形用户界面自动化库。它包括控制键盘和鼠标、在屏幕上查找颜色和位图以及显示警报的功能。
~~~
[很多python游戏相关的博客](http://eyehere.net/category/python/)
[autopy github](https://github.com/msanders/autopy)

## chardet识别字符串编码
[介绍](http://blog.csdn.net/tianzhu123/article/details/8187470)

## cv2
~~~
mac下安装
brew install OpenCV
pip install opencv-python
~~~

## gevent
[gevent doc](http://www.gevent.org/contents.html)
[gevent tutorial](http://sdiehl.github.io/gevent-tutorial/)


## jieba-Python中文分词组件
[jieba首页、文档和下载 - Python中文分词组件 - 开源中国社区](http://www.oschina.net/p/jieba)
[Jieba Demo Site](http://jiebademo.ap01.aws.af.cm/extract)
[NLPIR汉语分词系统](http://ictclas.nlpir.org/newsDetail?DocId=393)

## memorpy-读写某个进程内存的python模块
### 简介
~~~
memorpy是一个读写某个进程内存的python模块，但是仅限于x86的windows，因为其是使用ctype的windows函数完成；
git地址 https://github.com/n1nj4sec/memorpy
~~~
### 用法举例
~~~
form memorpy import *

mw = MemWorker('mhmain') # 打开已经运行的进程  参数为进程名(去掉.exe)

ad = mw.Address(0x4) # 参数为整形
ad.read(4) # 读取出来的结果为整形

def get_locaiton(x, y):  # 获取人物在地图内的坐标的基地址
    i = 0x5000000
    while i < 0x5500000:
        if mw.Address(i).read(4) == x and mw.Address(i+4).read(4) == y:
            return i, i+368, i-368 一对足够了
        i += 4

def get_map(m=1001):  #1001长安 1096傲来  1111天宫。。。
    l = []
    i = 0xAA00000
    while i < 0xB500000:
        if mw.Address(i).read(4) == m:
            l.append(i)
        i += 4
    for k in l:
        if mw.Address(k).read(4) == m:
            return k
~~~

## PIL - Python平台事实上的图像处理标准库
[PIL安装记录，编译支持jpeg png - cn.popeye - ITeye技术网站](http://cn-popeye.iteye.com/blog/1236691/)
[python - PIL zip jpeg decoders not working on runtime but work on install/selftest - Stack Overflow](http://stackoverflow.com/questions/11271941/pil-zip-jpeg-decoders-not-working-on-runtime-but-work-on-install-selftest)

## psutil
~~~
是一个跨平台库(http://pythonhosted.org/psutil/)能够轻松实现获取系统运行的进程和系统利用率（包括CPU、内存、磁盘、网络等）信息。它主要用来做系统监控，性能分析，进程管理。它实现了同等命令行工具提供的功能，如ps、top、lsof、netstat、ifconfig、who、df、kill、free、nice、ionice、iostat、iotop、uptime、pidof、tty、taskset、pmap等。目前支持32位和64位的Linux、Windows、OS X、FreeBSD和Sun Solaris等操作系统.

读取cpu的整个信息
import psutil
psutil.cpu_times()  #显示cpu的整个信息
获取单项值
psutil.cpu_times() .user   #如果要只但看那个的话就在后边加上.xxx就行了
获取cpu的逻辑个数
psutil.cpu_count()
获取cpu的物理个数
psutil.cpu_count( logical=False )
读取内存信息
linux系统内存利用率信息涉及to-tal(内存总数)，used(以使用内存)，free(空闲内存)，buffers(缓冲使用数)
cache(缓存使用数)，swap(交换分区使用数)，分别使用psutil.virtual_memory()与psuti.swap_memory()方法获取
import psuti
mem = psuti.virtual_memory()  #获取内存的完整信息
mem.total    #获取内存总数
mem.free     #获取空闲的内存信息
获取swap分区信息
psutil.swap_memory()
读取磁盘参数
磁盘利用率使用psutil.disk_usage方法获取，磁盘IO信息包括read_count(读IO数)，write_count(写IO数)
read_bytes(IO写字节数)，read_time(磁盘读时间)，write_time(磁盘写时间),这些IO信息用psutil.disk_io_counters()
获取磁盘的完整信息
psutil.disk_partitions()
获取分区表的参数
psutil.disk_usage('/')   #获取/分区的状态
获取硬盘IO总个数
psutil.disk_io_counters()
获取单个分区IO个数
psutil.disk_io_counters(perdisk=True)    #perdisk=True参数获取单个分区IO个数
读取网络信息
网络信息与磁盘IO信息类似,涉及到几个关键点，包括byes_sent(发送字节数),byte_recv=xxx(接受字节数),
pack-ets_sent=xxx(发送字节数),pack-ets_recv=xxx(接收数据包数),这些网络信息用psutil.net_io_counters()
psutil.net_io_counters()   #获取网络总IO信息
psutil.net_io_counters(pernic=True)     #pernic=True输出网络每个接口信息
获取当前系统用户登录信息
psutil.users()
获取开机时间
import psutil, datetime
psutil.boot_time()    #以linux时间格式返回
datetime.datetime.fromtimestamp(psutil.boot_time ()).strftime("%Y-%m-%d %H: %M: %S") #转换成自然时间格式

系统进程管理
获取当前系统的进程信息,获取当前英语程序的运行状态,包括进程的启动时间,查看设置CPU亲和度,内存使用率,IO信息
socket连接,线程数等
获取进程信息
psutil模块在获取进程方面有很好的支持,使用psutil.pids()方法获取所有进程的PID,
使用psutil.Process()方法获取单个进程的名称,路径状态等
查看系统全部进程
psutil.pids()
查看单个进程
p = psutil.Process(2423) 
p.name()   #进程名
p.exe()    #进程的bin路径
p.cwd()    #进程的工作目录绝对路径
p.status()   #进程状态
p.create_time()  #进程创建时间
p.uids()    #进程uid信息
p.gids()    #进程的gid信息
p.cpu_times()   #进程的cpu时间信息,包括user,system两个cpu信息
p.cpu_affinity()  #get进程cpu亲和度,如果要设置cpu亲和度,将cpu号作为参考就好
p.memory_percent()  #进程内存利用率
p.memory_info()    #进程内存rss,vms信息
p.io_counters()    #进程的IO信息,包括读写IO数字及参数
p.connectios()   #返回进程列表
p.num_threads()  #进程开启的线程数
听过psutil的Popen方法启动应用程序，可以跟踪程序的相关信息
from subprocess import PIPE
p = psutil.Popen(["/usr/bin/python", "-c", "print('hello')"],stdout=PIPE)
p.name()
p.username()
~~~

## pyenv - 管理python多个版本

## Pyinstaller - 将python打包为EXE文件
~~~
Pyinstaller将python打包为EXE文件
将python脚本打包为EXE文件有不少选择方案，本方案使用pyinstaller。
~~~

1. 第一步：
当然是到python的官网下载python并安装。编写好python脚本并且运行正常。

2. 第二步：
到http://www.pyinstaller.org/下载最新版本的pyinstaller。本文写作时是pyinstaller 2.0，
支持python 2.3～2.7版本。下载后解压到某个文件夹，如：c:\pyinstaller。

3. 第三步：
pyinstaller依赖一些windows的组件，需要到http://sourceforge.net/projects/pywin32/
下载相应python的版本，例如：python2.7的32位版：pywin32-218.win32-py2.7.exe

4. 第四步：
进入pyinstaller目录，试运行看看是否安装正确。
以下是运行屏幕：
~~~
C:\>cd c:\pyinstaller
C:\pyinstaller>c:\python27\python pyinstaller.py
Error: PyInstaller for Python 2.6+ on Windows needs pywin32.
Please install from http://sourceforge.net/projects/pywin32/
#上面的错误提示安装pywin32-218.win32-py2.7.exe。

C:\pyinstaller>c:\python27\python pyinstaller.py
Usage: python pyinstaller.py [opts] <scriptname> [ <scriptname> ...] | <specfile>
pyinstaller.py: error: Requires at least one scriptname file or exactly one .spec-file
# 以上信息表示可以开展工作了。

# 以下是测试一个python-tools.py文件的打包，文件放在当前目录的mytools文件夹里面
C:\pyinstaller>c:\python27\python pyinstaller.py --console --onefile  mytools\python-tools.py
67 INFO: wrote C:\pyinstaller\python-tools\python-tools.spec
175 INFO: Testing for ability to set icons, version resources...
224 INFO: ... resource update available
234 INFO: UPX is not available.
3179 INFO: checking Analysis
3179 INFO: building Analysis because out00-Analysis.toc non existent
3179 INFO: running Analysis out00-Analysis.toc
3189 INFO: Adding Microsoft.VC90.CRT to dependent assemblies of final executable
3218 INFO: Searching for assembly x86_Microsoft.VC90.CRT_1fc8b3b9a1e18e3b_9.0.21022.8_x-ww ...
3218 INFO: Found manifest C:\WINDOWS\WinSxS\Manifests\x86_Microsoft.VC90.CRT_1fc8b3b9a1e18e3b_9.0.21022.8_x-ww_d08d0375.manifest
3228 INFO: Searching for file msvcr90.dll
3228 INFO: Found file C:\WINDOWS\WinSxS\x86_Microsoft.VC90.CRT_1fc8b3b9a1e18e3b_9.0.21022.8_x-ww_d08d0375\msvcr90.dll
3228 INFO: Searching for file msvcp90.dll
3228 INFO: Found file C:\WINDOWS\WinSxS\x86_Microsoft.VC90.CRT_1fc8b3b9a1e18e3b_9.0.21022.8_x-ww_d08d0375\msvcp90.dll
3238 INFO: Searching for file msvcm90.dll
3238 INFO: Found file C:\WINDOWS\WinSxS\x86_Microsoft.VC90.CRT_1fc8b3b9a1e18e3b_9.0.21022.8_x-ww_d08d0375\msvcm90.dll
3648 INFO: Analyzing C:\pyinstaller\support\_pyi_bootstrap.py
6486 INFO: Analyzing C:\pyinstaller\PyInstaller\loader\archive.py
6855 INFO: Analyzing C:\pyinstaller\PyInstaller\loader\carchive.py
7242 INFO: Analyzing C:\pyinstaller\PyInstaller\loader\iu.py
7329 INFO: Analyzing mytools\python-tools.py
8355 INFO: checking Tree
8365 INFO: building because out00-Tree.toc missing or bad
8365 INFO: building Tree out00-Tree.toc
8894 INFO: checking Tree
8894 INFO: building because out01-Tree.toc missing or bad
8905 INFO: building Tree out01-Tree.toc
9304 INFO: Hidden import 'encodings' has been found otherwise
9315 INFO: Looking for run-time hooks
9315 INFO: Analyzing rthook C:\pyinstaller\support/rthooks/pyi_rth_encodings.py
9585 INFO: Analyzing rthook C:\pyinstaller\support/rthooks/pyi_rth_Tkinter.py
10569 INFO: Adding Microsoft.Windows.Common-Controls to dependent assemblies offinal executable
11896 INFO: Warnings written to C:\pyinstaller\python-tools\build\pyi.win32\python-tools\warnpython-tools.txt
11906 INFO: checking PYZ
11906 INFO: rebuilding out00-PYZ.toc because out00-PYZ.pyz is missing
11917 INFO: building PYZ out00-PYZ.toc
13763 INFO: checking PKG
13774 INFO: rebuilding out00-PKG.toc because out00-PKG.pkg is missing
13774 INFO: building PKG out00-PKG.pkg
19944 INFO: checking EXE
19944 INFO: rebuilding out00-EXE.toc because python-tools.exe missing
19944 INFO: building EXE from out00-EXE.toc
19974 INFO: Appending archive to EXE C:\pyinstaller\python-tools\dist\python-tools.exe

# 上面编译出来的exe能够正常运行了，但带一个黑色的console，以下重新编译，加入--windowed --icon，取消--console
C:\pyinstaller>c:\python27\python pyinstaller.py --windowed --onefile  -F --icon="c:\pyinstaller\mytools\ico.ico" mytools\python-tools.py

69 INFO: wrote C:\pyinstaller\python-tools\python-tools.spec
119 INFO: Testing for ability to set icons, version resources...
139 INFO: ... resource update available
149 INFO: UPX is not available.
2312 INFO: checking Analysis
2422 INFO: checking PYZ
2462 INFO: checking PKG
2542 INFO: building because C:\pyinstaller\python-tools\build\pyi.win32\python-tools\python-tools.exe.manifest changed
2542 INFO: building PKG out00-PKG.pkg
6468 INFO: checking EXE
6478 INFO: rebuilding out00-EXE.toc because python-tools.exe missing
6478 INFO: building EXE from out00-EXE.toc
6488 INFO: SRCPATH [('c:\\pyinstaller\\mytools\\ico.ico', None)]
6488 INFO: Updating icons from ['c:\\pyinstaller\\mytools\\ico.ico'] to c:\docume~1\user\locals~1\temp\tmpimsedq
6498 INFO: Writing RT_GROUP_ICON 0 resource with 20 bytes
6498 INFO: Writing RT_ICON 1 resource with 2216 bytes
6528 INFO: Appending archive to EXE C:\pyinstaller\python-tools\dist\python-tools.exe

# 好了，大功告成。如果无法执行exe，那么在cmd下执行exe，查看错误。
~~~

## python select socket
[详细讲解了epoll和select](http://www.cnblogs.com/vilyLei/articles/2749241.html)
[详细讲解了epoll和select](http://www.cnblogs.com/coser/archive/2012/01/06/2315216.html)

## python-docx word文档操作
[python-docx — python-docx 0.8.5 documentation](http://python-docx.readthedocs.org/en/latest/)

## Scrapy python爬虫框架
[Scrapy 0.24.4 文档](http://scrapy-chs.readthedocs.org/zh_CN/0.24/intro/tutorial.html)

## selenium 自动化测试框架(也可以用来做爬虫)
[selenium 2.46.0 : Python Package Index](https://pypi.python.org/pypi/selenium)
[如何控制滚动条到底部](http://m.bianceng.cn/Programming/extra/201409/44905.htm)
[python+Selenium2+chrome构建动态网页爬虫工具](http://blog.csdn.net/cjsafty/article/details/9206323)
[轻松自动化---selenium-webdriver](http://www.cnblogs.com/fnng/p/3258946.html)
[How to get selenium and xvfb to work in ubuntu - Stack Overflow](http://stackoverflow.com/questions/19662732/how-to-get-selenium-and-xvfb-to-work-in-ubuntu)


## SQLAlchemy python orm事实上的标准框架
[SQLAlchemy官网教程](http://docs.sqlalchemy.org/en/latest/orm/tutorial.html)
[Python ORM框架SQLAlchemy数据添加和事务回滚介绍](http://www.jb51.net/article/50896.htm)
[sqlacodegen - feihuadao的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/feihuadao/article/details/41943039)
[sqlacodegen - feihuadao的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/feihuadao/article/details/41943039)
[Query API — SQLAlchemy 1.0 Documentation](http://docs.sqlalchemy.org/en/rel_1_0/orm/query.html#the-query-object)

### 简单用法
~~~
session就是一个数据库连接；

session.add(u) 数据不会提交到数据库；

但是查询session.query()会把这条数据查询出来，且autoincrement的数据字段在数据已经被占据了(假如id在数据库最大为10，
现在sqlalchemy add(u)之后，在数据库插入一条数据id会是11，但是如果add(u)且查询出u之后，id不可能是11了只可能是12，
说明sqlalchemy在查询时会将前面的sql提交到数据库但是不会更新数据的数据但是会占据自增的字段保持id的唯一性)；
因此如果在一个api中有多次的插入，且后面的插入的数据依赖前面插入的且是数据库自动生成的数据时，可以一直add，如果哪一此失败了那么就rollback，同样可以保证这个api的幂等性；

session.rollback()是把session的数据清除了；

session.flush()就是把客户端尚未发送到数据库服务器的SQL语句发送过去,之后才能在这个Session中看到效果;

commit就是告诉数据库服务器提交事务,之后才能从其它Session中看到效果;


测试代码 驱动代码简单 测试时自己简单写一写就行，注意要对照数据库的数据
#coding:utf8
from sqlalchemy import Column, String, create_engine, Integer
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 创建对象的基类:
Base = declarative_base()

# 定义User对象:
class User(Base):
    # 表的名字:
    __tablename__ = 'user'

    # 表的结构:
    id = Column(Integer, primary_key=True)
    name = Column(String(45))

# 初始化数据库连接:
engine = create_engine('mysql+mysqlconnector://root:password@localhost:3306/test03')
Base.metadata.create_all(engine)
# 创建DBSession类型:
DBSession = sessionmaker(bind=engine)

session = DBSession()
~~~

## threading 标准库线程
[Python中threading的join和setDaemon的区别及用法［例子］ - - 博客频道 - CSDN.NET](http://blog.csdn.net/zhangzheng0413/article/details/41728869)
[Python线程指南](http://www.cnblogs.com/huxi/archive/2010/06/26/1765808.html)
[python的_threading_local模块 - x86的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/x86/article/details/4188292)

## Tornado python web服务器
~~~
tornado对wsgi支持很弱，flask是wsgiweb框架；
tornado虽然是异步框架，但是并不是所有可阻塞的操作都会进行异步操作，只会对tornado内部的操作做异步处理；
至于向数据取请求/time.sleep/urlopen等操作不会做异步处理，具体操作的异步查看下面的链接
~~~
[Tornado的源码](http://www.nowamagic.net/academy/detail/13321002)
[中文文档](http://demo.pythoner.com/itt2zh/index.html)
[中文文档](http://www.tornadoweb.cn/documentation)
[针对其他任务的异步客户端库](https://github.com/facebook/tornado/wiki/Links)
[异步的调用MongoDB服务器](https://github.com/bitly/asyncmongo)

## virturalenv Python运行环境隔离
### 我的理解
~~~
查看activate文件可知，virtualenv的机制就是将virtualenv/bin目录将入到path环境变量的开始位置，即shell中首先查找并使用virtualenv/bin下的python、pip等命令；
python模块查找(sys.path)也是变成了virtualenv/lib等，至于Python的sys.path的默认值问题再下面连接中
~~~
[PYTHONHOME](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONHOME)
[sys.path](https://docs.python.org/3/library/sys.html#sys.path)
### 相关链接
[Python 虚拟环境：Virtualenv](http://liuzhijun.iteye.com/blog/1872241)
[解决virtualenv下安装Python PIL的support not available问题](http://wangye.org/blog/archives/752/)
### install
~~~
apt-get install python-virtualenv
~~~

## alembic sqlalchemy ORM数据库模型版本管理
[中文简单使用教程](http://blog.csdn.net/deerlux/article/details/50181997)

## numpy
~~~
NumPy is the fundamental package for scientific computing with Python. It contains among other things:
a powerful N-dimensional array object
sophisticated (broadcasting) functions
tools for integrating C/C++ and Fortran code
useful linear algebra, Fourier transform, and random number capabilities
Besides its obvious scientific uses, NumPy can also be used as an efficient multi-dimensional container of generic data. 
Arbitrary data-types can be defined. This allows NumPy to seamlessly and speedily integrate with a wide variety of databases.
~~~
[官网教程](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html)
### Windows安装numpy
[下载对应的numpy版本](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy)
将whl后缀修改为zip, 解压, 将numpy复制到python/Lib/site-pakages/下














