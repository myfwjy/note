---
title: Proguard 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 定义
Proguard工具主要用于压缩、优化、混淆我们的代码，主作用可以用来移除代码中的无用类、字段、方法和属性同时可以混淆。发布时使用。
## 功能
压缩
检查移除没有用到的类、方法、属性。一些我们不想在发布时打包的代码，但是不想在项目中删掉的部分，可以在Proguard中标注移除。
优化
优化字节码文件，移除无用.class中的指令
混淆
混淆成无意义的名称，防止别人反编译之后读懂代码
预检测
在java平台上，对处理过的代码再次检测
## EntryPoint
EntryPoint在Proguard中标注不被处理的类和方法。Proguard会从上述EntryPoint中开始遍历，找到那些没有用的类和方法，在压缩阶段移除，那些非Entry Point的类、方法都会被设置为private、static或final，不使用的参数会被移除，此外，有些方法会被标记为内联的，在混淆的步骤中，ProGuard会对非Entry Point的类和方法进行重命名。