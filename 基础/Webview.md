---
title: Webview 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


## 内存泄漏
退出页面时要调用

``` java
protected void onDestroy() {
      super.onDestroy();
      mWebView.removeAllViews();
      mWebView.destroy()
}
```
## jsbridge
js和native端可以交互。
## webviewClient.onPageFinished->webviewClient.onPageChanged
前者在页面跳转时产生无数次。需要加载各种网页，最好用后者。
## 后台耗电
加载网页时自动开启线程，没有销毁的话，会一直在后台进行。暴力的退出方式`System.exit()`
## Webview硬件加速导致页面渲染问题
关闭硬件加速
## 为什么会造成内存泄漏
webview需要关联一个activity，但是webview内部执行操作又是在新的线程中，这个时间无法确认。那么有可能activity生命周期结束之后，webview依旧持有他的引用。

 1. 独立进程，涉及进程间通信
 2. 动态添加wevbview，对传入webview使用的context使用弱引用，动态添加的意思是创建一个ViewGroup来放置webview，使用的时候add进来，在activity销毁的时候移除