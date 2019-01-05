---
title: python小工具
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
---

1. [Python判断图片是否是jpeg格式（非扩展名方式）](http://outofmemory.cn/code-snippet/35749/python-test-jpeg-file-with-header-or-PIL-library) 

2. [python daemon 守护进程](http://blog.csdn.net/snleo/article/details/4410305)

3. [Python使用UUID库生成唯一ID](http://www.cnblogs.com/dkblog/archive/2011/10/10/2205200.html)

4. [linux下安装tesseract-ocr(图像识别)](http://blog.csdn.net/yoara/article/details/42392659)

5. 解压文件无乱码
~~~
#coding: utf8
import os
import sys
import zipfile
import chardet

print "Processing File " + sys.argv[1]

file = zipfile.ZipFile(sys.argv[1],"r");
for name in file.namelist():
    try:
        encoding = chardet.detect(name).get('encoding')
        print encoding
        utf8name=name.decode(encoding)
        #utf8name=name.decode('SHIFT_JIS')
    except Exception as e:
        print e
        utf8name = name
    print "Extracting " + utf8name
    pathname = os.path.dirname(utf8name)
    if not os.path.exists(pathname) and pathname!= "":
        os.makedirs(pathname)
    data = file.read(name)
    if not os.path.exists(utf8name):
        try:
            data = data.decode('SHIFT_JIS').encode('utf8')
        except:
            pass
        fo = open(utf8name, "w")
        fo.write(data)
        fo.close
file.close()
~~~