---
title: Binder 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## Linux内核的基础知识
进程隔离 / 虚拟地址空间-由于虚拟地址不同，避免线程A的数据写入线程B
系统调用-为了保护内核，应用程序只能访问部分许可资源
binder驱动-各个进程通过Binder内核进行交互的模块？
## Binder通信机制介绍
Android中有许多的跨进程通信，从性能上比socket更好，并且本身支持通信双方进行身份校验。
![enter description here][1]
## Aidl


  [1]: ./images/1511682018034.jpg