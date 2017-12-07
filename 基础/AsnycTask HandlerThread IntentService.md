---
title: AsnycTask HandlerThread IntentService
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## AsnycTask
**内部封装了一个Handler和线程池，可以灵活的在子线程和主线程之间切换**。
### 函数
Params, Progress, Result  传入参数类型，更新进度类型，返回结果类型。
onPreExecute()  执行前UI线程操作
doInBackground(Params... params)  子线程中操作，publishProgress(Progress... values)来更新进度。
onProgressUpdate(Progress... values)  调用publishProgress之后执行，UI线程。
onPostExecute(Result result)  执行结束后操作在UI线程。
### 注意
同样要注意内存泄漏的问题。非静态类持有外部类引用，可以通过多Activity持有弱引用，使用静态类，在destory函数中取消AsnycTask的执行解决。
AsnycTask不适合进行过于耗时的操作，不适合进行高并发操作。
如果屏幕横竖屏切换，或者被系统杀死，会造成数据丢失。
支持串行和并行执行，但是并行情况下不如串行安全稳定。

## HandlerThread
如果要进行耗时操作，需要启动子线程，但是频繁的创建和销毁很耗费资源。
本质是一个线程类，继承Thread。
**自带Looper**，可以进行Looper循环。
通过获取HandlerThread的Looper对象传递给Handler对象，可以在handleMessage方法中**执行异步任务**。
优点是不会又阻塞，减少了对性能的消耗。缺点是不能同时进行多任务的处理（messagequeue先进先出），处理效率较低。
与线程池注重并发不同，**HandlerThread是一个串行队列，背后只有一个线程。**

## IntentService
本质是封装了一个HandlerThread和Handler的异步框架。多次请求串行执行，执行结束自动关闭，不用手动stopSelf()。
