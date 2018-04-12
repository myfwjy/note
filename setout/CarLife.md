---
title: CarLife
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### ButterKnife
基于Java注解机制的辅助代码生成的框架。减少findViewById或者控件点击事件绑定、监听器、绑定指定资源（数组、图片）等。
需要注意的是，所有的绑定变量不可以是private、static类型的，取决于代码生成时，绑定使用的 classname.变量 的方式。如果绑定有逻辑顺序，请不要使用注解，所有的注解绑定在onCreate的bind(this)开始生效，必须在setContentView之后。
### zxing
### 图片压缩
### 启动其他音乐播放器
### ListView
### 与服务员数据同步