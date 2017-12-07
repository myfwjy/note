---
title: Rxjava2.0
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

给初学者的RxJava2.0教程(一)
http://www.jianshu.com/p/464fa025229e

 1. Rxjava和RxAndroid环境配置
 2. Observable、Observer、subscribe、ObservableEmitter、Disposable定义
 3. onNext、onComplete、onError事件
 4. subscribe重载方法

给初学者的RxJava2.0教程(二)
http://www.jianshu.com/p/8818b98c44e2

 1. 指定上下游工作线程
 2. Schedulers中的几种线程选择
 
 给初学者的RxJava2.0教程(三)
 http://www.jianshu.com/p/128e662906af
 

1. map 将圆形事件转换为矩形事件, 从而导致下游接收到的事件就变为了矩形
2. FlatMap(不确定顺序) 将一个发送事件的上游Observable变换为多个发送事件的Observables，然后将它们发射的事件合并后放进一个单独的Observable里.
3. concatMap（确实顺序）
 
 给初学者的RxJava2.0教程(四)
 http://www.jianshu.com/p/bb58571cdb64
 
1. Zip 通过一个函数将多个Observable发送的事件结合到一起，然后发送这些组合到一起的事件. 它按照严格的顺序应用这个函数。它只发射与发射数据项最少的那个Observable一样多的数据。
 
 给初学者的RxJava2.0教程(五)
 http://www.jianshu.com/p/0f2d6c2387c9
 
1. Backpressure
 
 给初学者的RxJava2.0教程(六)
 http://www.jianshu.com/p/e4c6d7989356
 
1. 通过降速和减少事件解决Backpressure
  
  给初学者的RxJava2.0教程(七)
  http://www.jianshu.com/p/9b1304435564
  
1. Flowable
2. Subscription.cancel()
3. Subscription request()

给初学者的RxJava2.0教程(八)
http://www.jianshu.com/p/a75ecf461e02
1. BackpressureStrategy.BUFFER 增大缓存
2. BackpressureStrategy.DROP 丢弃存不下的数据
3. BackpressureStrategy.LATEST 保留最新数据，总能获取到最后一个数据

给初学者的RxJava2.0教程(九)
http://www.jianshu.com/p/36e0f7f43a51
1. 通过request方法实现响应式拉，申请多少个数据，处理多少个数据
2. 上下游异步的情况下，每消费96个事件便会自动触发上游内部的request()去设置上游的requested的值