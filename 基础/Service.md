---
title: Service
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## Service基础
### Service是什么
Service是一种可以在后台执行长时间运行操作而没有用户界面的应用组件。如果是本地服务，由于运行在主线程，所以不能做耗时操作。
### 和Thread的区别
如果是本地服务，由于运行在主线程，所以不能做耗时操作，但是thread可以。Android中服务和后台和子线程是不同的概念，后台是指运行不依赖与UI线程，即使Activity被销毁了，这个后台进程依然存在。
Activity难以对子线程进行控制，特别是Activity销毁的时候，但是对Service就没有问题。
## 开启service的两种方式和区别
### startService
启动之后无限运行，与Activity生命周期无关，除非手动调用stopService(intent)。
注意onStartCommand()中的返回值，如果返回START_STICKY，系统会在内存不足服务被kill之后，内存充足时尝试重建服务，传入的intent值为空。
### bindService
可以多个Acivity都对其绑定，全部解绑定之后，自动销毁。
创建一个实现IBinder接口的实例对象并提供公共方法给客户端调用。从onBinder方法中返回这个实例，在onServiceConnected()方法中获取Binder，并使用提供的方法调用绑定服务。