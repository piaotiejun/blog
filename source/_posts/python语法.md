---
title: python语法
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- python
- python buildin
---


### 列表遍历删除
#### 结果做法
~~~
l = [0, 1, 2, 3, 4]

for i in l:
    print i, len(l)
    if i != 4:
        l.remove(i)
# 结果错误，因为remove之后l的长度发生变化，比如删除了0之后l变成了[1, 2, 3, 4]，
然后访问第二个元素直接变成了2，1被跳过
~~~
#### 正确做法
~~~
l = [0, 1, 2, 3, 4]

for i in l[:]:
    if i != 4:
        l.remove(i)
# 先生成一个l的副本，再对原来的list进行删除操作
~~~

### 矩阵转置行和列
~~~
matrix = [
[1, 2, 3, 4],
[5, 6, 7, 8],
[9, 10, 11, 12],
]

zip(*matrix)
~~~