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
通常意义上，Binder是一种跨进程通信机制。对于Server进程来讲，Binder指的是本地对象，对于Client来说，Binder是Binder代理对象。
![enter description here][1]
server向service manager注册自己的对象和add方法。
客户端向service manager请求服务端方法，service manager查询之后返回一个代理对象，包含add方法，但此方法为空。
客户端调用方法，将内容返回给内核。
内核根据返回调用具体的服务端方法。
服务端将结果经内核驱动返回给客户端。
## Aidl
Android Interface Definition Language
asInterface 同一进程直接返回Binder，不同进程返回代理Binder。
onTransact 判断客户端调用的方法是哪一个，然后实现本地调用，并返回。

  [1]: ./images/1511682018034.jpg