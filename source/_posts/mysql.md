---
title: mysql
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- 数据库
---

# Linux mysql 操作命令
1. mysql 启动等
~~~
安装mysql linux下的客户端: sudo apt-get install mysql-workbench

1.linux下启动mysql的命令：
/etc/init.d/mysql start 

2.linux下重启mysql的命令：
/ect/init.d/mysql restart 

3.linux下关闭mysql的命令：
/ect/init.d/mysql shutdown

4.连接本机上的mysql:
格式： mysql -h主机地址 -u用户名 －p用户密码
mysql -uroot -p

5.退出mysql命令：
exit/quit

6.修改mysql密码:
mysqladmin -u用户名 -p旧密码 password 新密码
或进入mysql命令行SET PASSWORD FOR root=PASSWORD("root");

7.增加新用户:
grant select on 数据库.* to 用户名@登录主机 identified by "密码"
如增加一个用户test密码为123，让他可以在任何主机上登录， 并对所有数据库有查询、插入、修改、删除的权限。首先用以root用户连入mysql，然后键入以下命令：
grant select,insert,update,delete on *.* to " Identified by "123";
~~~

2. 有关mysql数据库方面的操作
~~~
1、显示数据库列表。
show databases;
2、显示库中的数据表：
use 数据库名；
show tables;
3、显示数据表的结构：
describe 表名;
4、建库：
create database 库名;
GBK: create database test2 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
UTF8: CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
5、建表：
use 库名；
create table 表名(字段设定列表)；
6、删库和删表:
drop database 库名;
drop table 表名；
7、将表中记录清空：
delete from 表名;

truncate table  表名;
8、显示表中的记录：
select * from 表名;

9、编码的修改
如果要改变整个mysql的编码格式：  
启动mysql的时候，mysqld_safe命令行加入  
--default-character-set=gbk 

如果要改变某个库的编码格式：在mysql提示符后输入命令  
alter database db_name default character set gbk;

10.重命名表
alter table t1 rename t2;

11.查看sql语句的效率
 explain < table_name >
例如：explain select * from t3 where id=3952602;

12.用文本方式将数据装入数据库表中(例如D:/mysql.txt)
mysql> LOAD DATA LOCAL INFILE "D:/mysql.txt" INTO TABLE MYTABLE;
~~~

3. 数据的导入导出
~~~
1、文本数据转到数据库中
文本数据应符合的格式：字段数据之间用tab键隔开，null值用来代替。例：
load data local infile "文件名" into table 表名;

2、导出数据库和表
mysqldump --opt news > news.sql（将数据库news中的所有表备份到news.sql文件，news.sql是一个文本文件，文件名任取。）
mysqldump --opt news author article > author.article.sql（将数据库news中的author表和article表备份到author.article.sql文件， author.article.sql是一个文本文件，文件名任取。）
mysqldump --databases db1 db2 > news.sql（将数据库dbl和db2备份到news.sql文件，news.sql是一个文本文件，文件名任取。）
mysqldump -h host -u user -p pass --databases dbname > file.dump
就是把host上的以名字user，口令pass的数据库dbname导入到文件file.dump中
mysqldump --all-databases > all-databases.sql（将所有数据库备份到all-databases.sql文件，all-databases.sql是一个文本文件，文件名任取。）

3、导入数据
mysql < all-databases.sql（导入数据库）
mysql>source news.sql;（在mysql命令下执行，可导入表）
~~~

4. 数据库文件
[数据库文件说明及导入导出](http://blog.csdn.net/liyanhui1001/article/details/7025083)

# 优化
[一个数据库优化总结](http://www.cnblogs.com/villion/archive/2009/07/23/1893765.html)
[mysql官网explain详解](http://dev.mysql.com/doc/refman/5.7/en/explain-output.html)

1. 慢日志
~~~
linux下的mysql配置文件是my.cnf，一般会放在/etc/my.cnf，/etc/mysql/my.cnf

mysql下执行查看是否开启了慢日志查询：
show variables like '%quer%';

my.conf中的配置
[mysqld]
slow_query_log = ON  # 开启慢日志
slow-query-log-file = /var/log/mysql/slowquery.log  # 日志路径
long_query_time = 0.001  # 单位是秒
log-queries-not-using-indexes  # 查看没有使用索引的查询


慢查询日志文件的信息格式：

# Time: 160817 23:44:54
# User@Host: root[root] @ localhost []
# Query_time: 0.000317  Lock_time: 0.000087 Rows_sent: 5  Rows_examined: 19   #查询时间 返回结果行数  查询行数
SET timestamp=1471448694;
SELECT /* ResourceLoader::preloadModuleInfo  */  md_module,md_deps  FROM `my_wikimodule_deps`   WHERE md_skin = 'modern';
~~~

2. 数据库优化
~~~
11、联合字符或者多个列(将列id与":"和列name和"="连接)
select concat(id,':',name,'=') from students;

12、limit(选出10到20条)<第一个记录集的编号是0>
select * from students order by id limit 9,10;

13、MySQL不支持的功能
事务，视图，外键和引用完整性，存储过程和触发器


14、MySQL会使用索引的操作符号
<,<=,>=,>,=,between,in,不带%或者_开头的like

15、使用索引的缺点
1)减慢增删改数据的速度；
2）占用磁盘空间；
3）增加查询优化器的负担；
当查询优化器生成执行计划时，会考虑索引，太多的索引会给查询优化器增加工作量，导致无法选择最优的查询方案；

16、分析索引效率
方法：在一般的SQL语句前加上explain；
分析结果的含义：
1）table：表名；
2）type：连接的类型，(ALL/Range/Ref)。其中ref是最理想的；
3）possible_keys：查询可以利用的索引名；
4）key：实际使用的索引；
5）key_len：索引中被使用部分的长度（字节）；
6）ref：显示列名字或者"const"（不明白什么意思）；
7）rows：显示MySQL认为在找到正确结果之前必须扫描的行数；
8）extra：MySQL的建议；

17、使用较短的定长列
1）尽可能使用较短的数据类型；
2）尽可能使用定长数据类型；
a）用char代替varchar，固定长度的数据处理比变长的快些；
b）对于频繁修改的表，磁盘容易形成碎片，从而影响数据库的整体性能；
c）万一出现数据表崩溃，使用固定长度数据行的表更容易重新构造。使用固定长度的数据行，每个记录的开始位置都是固定记录长度的倍数，可以很容易被检测到，但是使用可变长度的数据行就不一定了；
d）对于MyISAM类型的数据表，虽然转换成固定长度的数据列可以提高性能，但是占据的空间也大；

18、使用not null和enum
尽量将列定义为not null，这样可使数据的出来更快，所需的空间更少，而且在查询时，MySQL不需要检查是否存在特例，即null值，从而优化查询；
如果一列只含有有限数目的特定值，如性别，是否有效或者入学年份等，在这种情况下应该考虑将其转换为enum列的值，MySQL处理的更快，因为所有的enum值在系统内都是以标识数值来表示的；

19、使用optimize table
对于经常修改的表，容易产生碎片，使在查询数据库时必须读取更多的磁盘块，降低查询性能。具有可变长的表都存在磁盘碎片问题，这个问题对blob数据类型更为突出，因为其尺寸变化非常大。可以通过使用optimize table来整理碎片，保证数据库性能不下降，优化那些受碎片影响的数据表。 optimize table可以用于MyISAM和BDB类型的数据表。实际上任何碎片整理方法都是用mysqldump来转存数据表，然后使用转存后的文件并重新建数据表；

20、使用procedure analyse()
可以使用procedure analyse()显示最佳类型的建议，使用很简单，在select语句后面加上procedure analyse()就可以了；例如：
select * from students procedure analyse();
select * from students procedure analyse(16,256);
第二条语句要求procedure analyse()不要建议含有多于16个值，或者含有多于256字节的enum类型，如果没有限制，输出可能会很长；

21、使用查询缓存
1）查询缓存的工作方式：
第一次执行某条select语句时，服务器记住该查询的文本内容和查询结果，存储在缓存中，下次碰到这个语句时，直接从缓存中返回结果；当更新数据表后，该数据表的任何缓存查询都变成无效的，并且会被丢弃。
2）配置缓存参数：
变量：query_cache _type，查询缓存的操作模式。有3中模式，0：不缓存；1：缓存查询，除非与 select sql_no_cache开头；2：根据需要只缓存那些以select sql_cache开头的查询； query_cache_size：设置查询缓存的最大结果集的大小，比这个值大的不会被缓存。

22、调整硬件
1）在机器上装更多的内存；
2）增加更快的硬盘以减少I/O等待时间；
寻道时间是决定性能的主要因素，逐字地移动磁头是最慢的，一旦磁头定位，从磁道读则很快；
3）在不同的物理硬盘设备上重新分配磁盘活动；
如果可能，应将最繁忙的数据库存放在不同的物理设备上，这跟使用同一物理设备的不同分区是不同的，
因为它们将争用相同的物理资源（磁头）。
~~~

# Mysql生成数据库文档
~~~
#!/bin/bash

set -x;

function linuxs {
    mysql  recruit -u root  -p1990221  -e "
    select
        a.table_name, b.table_comment, a.column_name, a.column_type, a.column_comment
    from
        information_schema.columns a join information_schema.tables b on a.table_name=b.table_name and a.table_schema=b.table_schema
    where
        b.table_schema='recruit'
    into
        outfile '/tmp/out.xls'
    "

    cp /tmp/out.xls .
    #iconv -f utf8 -t gb2312 out.xls > table_desc.xls
    #rm out.xls
    return 0
}

function mac {
    mysql  recruit -u root  -p1990221  -e "
    select
        a.table_name, b.table_comment, a.column_name, a.column_type, a.column_comment
    from
        information_schema.columns a join information_schema.tables b on a.table_name=b.table_name and a.table_schema=b.table_schema
    where
        b.table_schema='recruit'
    into
        outfile '/Users/potiejun/workspace/python_workspace/recruit/sql/out.xls'
    "

    iconv -f utf8 -t gb2312 out.xls > table_desc.xls
    rm out.xls
    return 0
}

system=$(uname -a | cut -d ' ' -f 1)
if [ $system = "Darwin" ]; then
    mac;
else
    linuxs;
fi
~~~

# mysql建立外键约束引用自身处理方法
~~~
当需要在在一张表中加入外键而且该外键就是本身这张表的主键时，无法插入第一条数据，因为这条数据的外键不能为空；
因此做法应该是先建立这张表不包含引用自身的外键，插入一条数据，再建立外键约束
~~~

