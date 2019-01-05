---
title: Opencv
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python模块
---

# 链接
[](http://mp.weixin.qq.com/s?__biz=MzA3MDExNzcyNA==&mid=402907292&idx=1&sn=889c4abcf576e24525ea6a705069c4de)
[](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html)
[](http://mp.weixin.qq.com/s?__biz=MzA3MDExNzcyNA==&mid=2650391990&idx=1&sn=a6f4607867441c60b00730afe53325a7#rd)

# windows安装opencv
~~~
安装opencv依赖numpy

http://ufpr.dl.sourceforge.net/project/opencvlibrary 下去找到对应的版本
http://ufpr.dl.sourceforge.net/project/opencvlibrary/opencv-win/3.1.0/opencv-3.1.0.exe

执行exe；
将D:\opencv\opencv\build\python\2.7\x64\cv2.pyd 复制到python的site-packages下；
完成
~~~

# 判断一张图片是否为另一张图片的子图
~~~
#!/usr/bin/python
# coding: utf-8
import cv2
import numpy as np
import sys

def in_images(source_img, template_img):
    img = cv2.imread(source_img, 0)
    template = cv2.imread(template_img, 0)
    w, h = template.shape[::-1]
    res = cv2.matchTemplate(img, template, cv2.TM_SQDIFF_NORMED)
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)
    if max_val >= 1:
        print 'success'
        return True
    print 'failed'
    return False
~~~