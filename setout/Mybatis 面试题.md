---
title: Mybatis 面试题
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### ${} #{}区别
$是变量占位符，比如说传递表格的名称，是一种静态的文本转换。#是参数占位符，sql语句中会是？代替，然后从preparedStatement中查找参数替换上去，如果是字符串还是加上“”。