---
title: js基本知识
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- js
---

# Js相关名词
~~~
Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。
~~~

[关于后端程序员写前端用什么框架更好](https://www.zhihu.com/question/37946473)

# npm
[npm及package.json详细介绍](https://blog.csdn.net/u011240877/article/details/76582670)

# 拷贝
1. js浅拷贝
~~~
浅拷贝：拷贝父对象，不会拷贝对象的内部的子对象。即拷贝列表name里面的一级元素的内存地址，不拷贝name里的小列表里的元素的内存地址。

let a = [1, 2, 3];
let b = [...a];   // 浅拷贝

let a = {'s': 1};
let b = {...a};  // 浅拷贝
~~~
2. js深拷贝
~~~
深拷贝：完全拷贝了父对象及其子对象。

function deepClone(obj) {
    let _obj = JSON.stringify(obj);
    let objClone = JSON.parse(_obj);

    return objClone;
} 

let a=[0,1,[2,3],4], b=deepClone(a);  // 深拷贝
a[0]=1;
a[2][0]=1;
console.log(a,b);
~~~

# type script
[type script官网教程](https://www.tslang.cn/docs/home.html)
1. TypeScript介绍
~~~
TypeScript 扩展了JavaScript语法，任何已经存在的JavaScript程序，可以不加任何改动，在TypeScript环境下运行。
TypeScript只是向JavaScript添加了一些新的遵循ES6规范的语法，以及基于类的面向对象编程的这种特性。

其次，2016年9月底发布的Angular2框架，这个框架本身是由TypeScript编写的。Angular框架，大家都知道，它是由谷歌公司开发的，非常流行的框架。
也就是说，现在TS这门语言是由微软和谷歌这两大公司在背后支持。因此我们有理由相信，在未来一段时间内，TS有可能成为前端开发语言中的主流。

微软开发的一门编程语言
JavaScript的超集
遵循最新的ES6规范
TypeScrip优势
支持ES6规范：2015年发布的，它指出了未来一段时间内，客户端脚本语音的发展方向。
强大的IDE支持：体现在三个特性上，1.类型检查，在TS中允许你为变量指定类型。2.语法提示。3.重构。
Angular2的开发语言
~~~
