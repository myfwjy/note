---
title: View绘制机制 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

自上而下的绘制视图。
measure  确定视图大小
layout  确定视图位置
draw  绘制视图

## measure
自定义时需要重写onMeasure。
LayoutParams 视图的大小，对应xml中的值，可以使具体值，也可以是wrap_content和match_parent。
MeasureSpec 测量规格。是一个32为int值，前两位表示mode，后30为表示在该模式下的size值。
UPSPECIFIED : 父容器对于子容器没有任何限制,子容器想要多大就多大
EXACTLY: 父容器已经为子容器设置了尺寸,子容器应当服从这些边界,不论子容器想要多大的空间。
AT_MOST：子容器可以是声明大小内的任意大小
整个measure的过程就是，父视图通过自己的MeasureSpec和子视图的LayoutParams来确定子视图的MeasureSpec，通过子视图的MeasureSpec确定子视图的宽和高，然后不断传递下去，最后确定整体视图的大小。
参考：http://www.jianshu.com/p/5a71014e7b1b

## layout
自上而下分析视图位置。

## draw
绘制背景->对view的内容进行绘制->对当前view的所有子view进行绘制->对View的滚动条进行绘制
invalidate  当前视图被重绘
requestLayout 当前视图的onLayout onMeasure被调用，如果大小影响了父视图和兄弟视图，他们的onLayout onMeasure也会被调用。requestLayout触发onDraw可能是因为在在layout过程中发现l,t,r,b和以前不一样，那就会触发一次invalidate，所以触发了onDraw，也可能是因为别的原因导致mDirty非空（比如在跑动画）
参考：http://blog.csdn.net/litefish/article/details/52859300
