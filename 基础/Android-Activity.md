---
title: Android-Activity
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## Activity的生命周期
### 四种状态
runing / paused / stopped / killed
### 生命周期分析
Activity启动->onCreate()->onStart()->onResume()
点击Home回到主界面（Activity不可见）->onPause()->onStop()
回到Activity->onRestart()->onStart()->onResume()
退出当前Activity->onPause()->onStop()->onDestory()
### Android进程的优先级
前台：当前和用户交互的Activity以及与之绑定的Service
可见：被部分遮盖的Activity
服务：启动一个Service后台运行
后台：Home键返回主界面后，刚刚的Activity转为后台，当内存不足时，会被系统回收
空：以上都不是，不包括活跃组件，用于保存缓存，可随时清理
## 任务栈
先进后出，与启动模式配合
## 启动模式

 1. standard
 2. singleTop
 3. singleTask
 4. singleInstance
 
 注意后面三种，在跳转到一个已经在任务栈中的Activity时，会在onRestart()之前调用onNewIntent()
## scheme跳转协议
android中的scheme是一种页面内跳转协议，是一种非常好的实现机制，通过定义自己的scheme机制，可以非常方便的跳转app中的各个页面；通过scheme协议，服务器可以定制化告诉App跳转到哪个页面，可以通过通知栏定制跳转到哪个页面，可以通过H5定制化跳转页面等。