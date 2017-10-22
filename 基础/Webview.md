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
