---
title: shell知识
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- shell
---

# linux常用命令

[linux命令查找](http://linux.51yip.com/)
[linux命令行常用光标移动快捷键](https://www.cnblogs.com/aslongas/p/5899586.html)

# 常识
~~~
linux帮助文档中的[]代表可选参数 {}代表必选参数
sh test.sh # 开启一个子进程 执行test.sh test.sh运行结束回归到父shell
. test.sh # 在本进程内执行test.sh 等价于source test.sh
./test.sh 相当于 sh test.sh
~~~

# TODO
~~~
区分交互式和非交互式shell、登录和非登录shell之间不同
ssh* 把*补全
screen
tmux 终端中多显示器
openssh 生成https认证
cheat
~~~

# 常用命令
~~~
lsb_release - print distribution-specific information
md5sum - compute and check MD5 message digest
nc — arbitrary TCP and UDP connections and listens
ulimit - get and set user limits
top - display Linux processes
curl - transfer a URL
ps - report a snapshot of the current processes
pstree - display a tree of processes
pstack - print a stack trace of running processes 只在linux32bit有效
ln - make links between files
ssh*
openssl - OpenSSL command line tool
rsa - RSA key processing tool
tcpdump - dump traffic on a network
who - show who is logged on
tr - translate or delete characters
export - 将变量放入环境中 export -p查看当前环境
unset - 删除shell中的变量、函数
env -- set and print environment env ls=/bin/ls ls . ls=/bin/ls只对ls .有效 环境变量不会改变
vipw 查看修改删除用户信息
useradd usermod 系列
service --status-all 查看服务状态
ubuntu下设置path 需要修改envirment
cat /proc/cpuinfo 查看linux-cpu

http://www.91ri.org/tag/vsftpd
http://quxiao.github.io/blog/2012/05/13/add-startup-programe-with-expect/
~~~

# ubutnu修改源
1. vim /etc/apt/source.list  将原来的列表删除，添加如下内容
~~~
deb http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
~~~
2. 运行sudo apt-get update
3. 运行sudo apt-get upgrade


