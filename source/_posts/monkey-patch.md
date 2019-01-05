---
title: monkey patch
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python规范与协议
---

[转载自 mattkang的博客-什么是猴子补丁(monkey patch)](http://blog.csdn.net/handsomekang/article/details/40297775?utm_source=tuicool&utm_medium=referral)

### 说明
~~~
monkey patch指的是在运行时动态替换,一般是在startup的时候.
用过gevent就会知道,会在最开头的地方gevent.monkey.patch_all();
把标准库中的thread/socket等给替换掉.这样我们在后面使用socket的
时候可以跟平常一样使用,无需修改任何代码,但是它变成非阻塞的了.
举例，工程代码中很多地方用的import json,现在想替换成ujson,
难道几十个文件要一个个把import json改成import ujson as json吗?
其实只需要在进程startup的地方monkey patch就行了.是影响整个进程空间的.
同一进程空间中一个module只会被运行一次.
~~~

### 示例代码
~~~
import json  
import ujson  
def monkey_patch_json():  
    json.__name__ = 'ujson'  
    json.dumps = ujson.dumps  
    json.loads = ujson.loads  

monkey_patch_json()  
print 'main.py',json.__name__ 
# 运行main.py,可以看到都是输出'ujson',说明后面import的json是被patch了的.
# 最后,注意不能单纯的json = ujson来替换.
~~~
