---
title: Gradle
date: 2019-01-05 22:06:55
updated: 2019-01-05 22:06:55
tags:
categories:
- java
---

# 链接
[gradle中文教程链接](https://www.w3cschool.cn/gradle_user_guide/)

# 简介
~~~
Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化构建开源工具。
它使用一种基于Groovy的特定领域语言(DSL)来声明项目设置，抛弃了基于XML的各种繁琐配置。
面向Java应用为主。当前其支持的语言限于Java、Groovy、Kotlin和Scala，计划未来将支持更多的语言。
~~~

# gradle wrapper
~~~
Gradle Wrapper的作用是简化Gradle本身的安装、部署。
不同版本的项目可能需要不同版本的Gradle，手工部署的话比较麻烦，而且可能产生冲突，所以需要Gradle Wrapper帮你搞定这些事情。
Gradle Wrapper是Gradle项目的一部分。

在idea、android studio中新建一个工程时，ide会帮我们构建一个gradle/wrapper目录，
项目初始化时会根据gradle/wrapper目录中的配置去下载对应版本的gradle，再使用下载下来的gradle来构建工程。
~~~
[彻底搞懂Gradle、Gradle Wrapper与Android Plugin for Gradle的区别和联系](https://www.cnblogs.com/jiangxinnju/p/8229129.html)

# Projects 和 tasks
~~~
Gradle 里的任何东西都是基于这两个基础概念:
projects(项目): project是一个抽象的概念，代表一件要做的事(我理解为一个顺序执行的task列表,就是一个shell),  比如部署你的应用. 每一个 project 是由一个或多个 tasks 构成的.
tasks(任务): 一个 task 代表一些更加细化的任务，比如复制一个文件。Gradle tasks 和 Ant 的 targets 是对等的.
~~~
## tasks
~~~
// 定义任务 写法1
task hello {
    doFirst {
        println 'hello first1'
    }
    doLast {
        println 'Hello world1!'
    }
}

// 定义任务 写法2: << 操作符是 doLast 的简单别称
task hello1 << {
    println 'Hello world2!'
}

// 任务依赖
task intro(dependsOn: hello) << {
    println "I'm Gradle1"
}

// 任务依赖
task intro1(dependsOn: "hello1") << {
    println "I'm Gradle2"
}

// 动态任务
4.times { counter ->
    task "task$counter" << {
        println "I'm task number $counter"
    }
}

// 创建额外的依赖
task0.dependsOn task2, task3

// 给一个已经存在的任务加入行为
hello.doFirst {
    println 'Hello Venus'
}

// 访问一个任务的属性
hello.doLast {
    println "Greetings from the $hello.name task."
}

// 给任务加入自定义的属性
task myTask {
    ext.myProperty = "myValue"
}
task printTaskProperties << {
    println myTask.myProperty
}

// 调用ant: Groovy 自带了一个 AntBuilder.
// 相比于从一个 build.xml 文件中使用 Ant 任务, 在 Gradle 里使用 Ant 任务更为方便和强大.

// 默认任务
defaultTasks 'hello', 'printTaskProperties'
~~~

# 执行方式
~~~
命令行下执行:
./gradlew -q taskname
./gradlew -q  // 如果设置了默认任务
~~~

