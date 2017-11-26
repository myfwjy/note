---
title: Broadcast Receiver
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 广播
一种广泛运用的在应用程序之间传递信息的机制，Android中我们要发送的广播内容是一个Intent，这个Intent中可以携带我们要传送的数据。
同一APP的不同进程不同组件之间；不停APP的组件之间消息通信。
普通广播Context.sendBroadcast；系统广播Context.sendOrderedBroadcast；只在自身APP传递的本地广播。
## 实现广播-receiver
静态注册，注册完成一直接受广播；动态注册，跟随Activity的生命周期。
## 广播实现机制
自定义广播接受者BroadcastReceiver，复写onReceive()
通过Binder机制向AMS（Activity Manager Service）进行注册
广播发送者通过Binder机制向AMS发送广播
AMS查找符合相应条件的（利用IntentFilter Permission）BroadcastReceiver，将广播发送到BroadcastReceiver（一般情况下是Activity）相应的消息循环队列中。
消息循环执行拿到此广播，回调BroadcastReceiver的onReceive()
## LocalBroadcast Manager