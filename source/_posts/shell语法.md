---
title: shell语法
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- shell
---

# shell变量
~~~
数据类型只有int、str、list
shell变量默认情况下保存一个str
使用一些命令例如let、declare、expr、双括号等使一个变量成为int或进行数学运算
在shell中，逻辑值表示程序的返回值，如果程序成功返回，则为真，如果不成功返回，则为假

变量名必须以字母或下划线字符开头，其余的字符可以是字母、数字(0~9)或下划线字符，任何其他的字符都标志着变量名的终止
名字是大小写敏感的。给变量赋值时，等号周围不能有任何空白符。为了给变量赋空值，可以在等号后跟一个换行符

shell变量可分为两类:
局部变量:
    只在创建它们的shell中可用
环境变量:
    可以在创建它们的shell及其派生出来的任意子进程中使用
~~~

1. 赋值取值算数
~~~
用set命令可以查看所有的变量，unset var命令可以清除变量var，readonly var可以把var变为只读变量，export var将var加入环境变量

name=piaotiejun # 赋值
dates=$(date) # 将date的返回值赋值给dates
name='piaotiejun is a pythoner' # 带空格的参数要加引号 加了引号后就是字符串
$name # 取值
${name} # 取值 最好以这种方式取值 无歧义
export name # 将name加入环境变量中
~~~

2. int
~~~
declare -i age=22 # 用declare把变量定义为整型
let age=${age}+1 # 可以用let命令使其进行数学运算 只支持int
$((10*(10+30))) # 数学运算 (())内为计算公式  只支持int
$[10*2] # $[]将中括号内的表达式作为数学运算先计算结果再输出 只支持int
~~~

3. str

### 替换运算符
~~~
${name:-piaotiejun} ==> name if name else piaotiejun
${name:=piaotijeun} ==> name if name else piaotijeun; name=piaotiejun
${name:?message} ==> name if name else raise message
${name:+mesage} ==> message if name else null

${name-piaotiejun} ==> name if exist(name) else piaotiejun
${name=piaotijeun} ==> name if exist(name) else piaotijeun; name=piaotiejun
${name?message} ==> name if exist(name) else raise message
${name+mesage} ==> message if exist(name) else null
~~~

### 匹配运算符
~~~
${var#pattern} 删除var开始位置处符合pattern的字符串
${var##pattern} 删除var开始位置处符合pattern的字符串(贪婪模式) 
${var%pattern} 删除var结束位置处符合pattern的字符串 
${var%%pattern} 删除var结束位置处符合pattern的字符串 (贪婪模式)
~~~

### 其他运算
~~~
${#var} 返回字符串的长度
$n 执行脚本的参数  -1<n<10
${n} 执行脚本的参数  n>-1
$# 执行脚本的参数个数
$*|$@ 执行脚本的所有参数 会将所有参数逐个传递给执行的命令(如果参数有空格 那个该参数将变为两个参数)
"$@" 执行脚本的所有参数 会将所有参数逐个传递给执行的命令(不忽略空格) 最好用这种方式传递参数
$? 前一个命令的退出状态
$$ shell进程id
$0 进程名称
$! 最后一个后台进程id

${var:index} 返回var第index个字符开始的字符串
${var:index1-index2} 返回var第index个字符开始的字符串 index=|index1-index2|
${var:index:length} 返回var第index个字符开始的长度为length的字符串 length=length if length>=0 else len(var)-index
~~~

4. bool
~~~
在shell中，逻辑值表示程序的返回值，如果程序成功返回，则为真，如果不成功返回，则为假
test命令可以用来进行数值测试，字符串测试，文件测试 test命令等价于[空格command空格] 代表一个命令执行的返回值，空格不能省略
[]可以进行与或运算 []&&[] []||[]
[[]]可以进行字符串比较正则比较 [[abc == ab* ]]
~~~

5. list
~~~
list=(abc cde def) # ()是数组定义符号
list=$(seq 1 2 10) # seq生成整形数组
{1..10} # 生成1-10的数组 无法赋值给变量
${list[index]} # 访问数组
${list[@]} # 访问数组全部元素
${list[*]} # 访问数组全部元素
${#list[@]} # 数组长度 只对()定义的数组生效
~~~

# 控制语句
1. if else
~~~
if [ command ]; then
  dosomething 
elif [ command ]; then
  dosomethng
else
  dosomething
fi
~~~

2. case
~~~
case var in
  pattern1)
    dosomething;;
  pattern2)
  dosomething;;
esac
~~~

3. for
~~~
for i in a b c
do
  echo $1
done

list=$(seq 10)
for i in ${list[@]}
do
  echo $i
done

d=${date}
list=($d abc)
for i in ${list[@]}
do
  echo $i
done

for ((i=0; i<10; i++)) # c风格for
{
    echo $i
}     
~~~

4. while
~~~
while []
do
  dosomething
done
~~~

5. until 与while相仿
~~~
until [] # false时继续
do
  dosomething
done
~~~

# 函数
~~~
#shell中函数相当于小型脚本 执行了包含某个函数的脚本后该函数可以在命令行直接调用
#可以接受参数{$n}(如果是list参数那么需要在函数内将${@}赋值给一个新变量，直接${list[@]只能取到list[0])
#可以有返回值(0-255,默认为函数中最后一条语句执行状态)

function test {
    echo 'in test func'
    local new_array
    new_array=($@)
    echo ${new_array[@]}
    for ((i=0; i<10; i++))
    {   
        echo ${i}
        echo ${new_array[$i]}
    }   

    return 0
}

seqs=$(seq 100 100 1000)
echo ${seqs[@]}
test "$seqs"; # 函数调用
echo test return $?
~~~
