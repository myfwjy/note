---
title: Kotlin语音特性--与Java比较
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### 变量定义
分只读类型和可变类型变量。val var
可以自动推断类型
对变量可否为null在定义时就确认

### 算术运算

### for 循环语句，while 循环语句
if表达式可以是代码块，最后的表达式作为该块的值
when取代switch
for循环类似c#的foreach，对list遍历有一些特例
返回跳转可以使用标签
https://www.kotlincn.net/docs/reference/control-flow.html

### 函数定义，函数调用
参数声明和返回值声明都后置，使用分号分割
允许参数是lambda表达式
可变数量参数
中缀表达法，成员函数或者扩展函数

### 递归
尾递归函数（调用自身是最后一步的函数）

### 静态类型系统
is可以进行类型检查
在is或者显示转换之后，Kotlin可以对一些内容进行智能转换
as可以进行不安全的类型转换

### 类型推导
自动分辨变量类型

### lambda 函数

### 面向对象

### 垃圾回收

### 指针算数

### 没有CE
http://www.yinwang.org/blog-cn/2017/05/23/kotlin

### 协程