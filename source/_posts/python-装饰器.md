---
title: python 装饰器
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python buildin
---

1. [Python装饰器学习](http://www.cnblogs.com/rhcad/archive/2011/12/21/2295507.html)

2. [Python装饰器与面向切面编程](http://www.cnblogs.com/huxi/archive/2011/03/01/1967600.html)

3. [python装饰器示例(很多)](https://wiki.python.org/moin/PythonDecoratorLibrary)

4. [闭包](https://zhuanlan.zhihu.com/p/21680710?refer=pythonpx)

### 闭包
内部函数引用外部函数的局部变量，这个内部函数就是闭包
~~~
def f():
    s = 10
    def warper():
        print s
    return warper

q = f()
f函数返回的是warper函数，warper函数也是一个对象，q=f()因此warper函数对象的引用计数不为0，s的引用计数也不为0，所以s的生命周期在q不被del之前都会存在，这就是闭包
~~~

