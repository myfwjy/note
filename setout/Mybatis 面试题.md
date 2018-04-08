---
title: Mybatis 面试题
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### ${} #{}区别
$是变量占位符，比如说传递表格的名称，是一种静态的文本转换。#是参数占位符，sql语句中会是？代替，然后从preparedStatement中查找参数替换上去，如果是字符串还是加上“”。
### dao接口方法重载
寻找mappedstatement是根据类全名+方法名唯一确定，所以不可以。
### dao工作原理
Dao接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Dao接口生成代理proxy对象，代理对象proxy会拦截接口方法，转而执行MappedStatement所代表的sql，然后将sql执行结果返回。
### 如何进行分页
使用rowBounds。
### 为什么说Mybatis是半自动ORM映射工具
Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成。
### Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？
使用resultmap或者select使用别名。
有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。