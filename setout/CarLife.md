---
title: CarLife
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### ButterKnife
基于Java注解机制的辅助代码生成的框架。减少findViewById或者控件点击事件绑定、监听器、绑定指定资源（数组、图片）等。
需要注意的是，所有的绑定变量不可以是private、static类型的，取决于代码生成时，绑定使用的 classname.变量 的方式。如果绑定有逻辑顺序，请不要使用注解，所有的注解绑定在onCreate的bind(this)开始生效，必须在setContentView之后。
### zxing
将github中zxing安卓部分添加到项目中，当然也可以做成jar包，需要二维码扫描的部分是车牌号的导入，所以需要使用到zxing相机二维码扫描的部分（本身还带有图片二维码扫描）。先进入QRCodeGuideActivity类，然后调用QRCodeCaptureActivity，在handleDecode方法中对扫码成功后的动作作处理，通过intent携带Bundle信息返回给项目类。
### 图片压缩
### 启动其他音乐播放器
### ListView
### 与服务员数据同步