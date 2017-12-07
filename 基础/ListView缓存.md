---
title: ListView缓存
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 定义
一个数据集合以动态滚动的方式展示到用户界面的view。

## Adapter
将数据转换成view交给ListView显示。

## RecycleBin
实现复用的关键类。
### ActiveView
ActiveView其实就是在UI屏幕上可见的视图(onScreenView)，也是与用户进行交互的View，那么这些View会通过RecycleBin直接存储到mActiveView数组当中，以便为了直接复用。
### ScrapView
那么当我们滑动ListView的时候，有些View被滑动到屏幕之外(offScreen) View，那么这些View就成为了ScrapView，也就是废弃的View，已经无法与用户进行交互了，这样在UI视图改变的时候就没有绘制这些无用视图的必要了。他将会被RecycleBin存储到mScrapView数组当中，但是没有被销毁掉，目的是为了二次复用，也就是间接复用。
### 复用机制
当新的View需要显示的时候，先判断mActiveView中是否存在，如果存在那么我们就可以从mActiveView数组当中直接取出复用，也就是直接复用（不需要重新赋值，不需要走getView函数），否则的话从mScrapView数组当中进行判断，如果存在，那么二次复用（复用view，调用getView函数，convertView不为空，重新赋值）当前的视图，如果不存在，那么就需要inflate View了（调用getView函数，convertView为空）。
**ListView始终只会在getView方法中inflate一页的Item，也就是new View只会执行一页Item的次数。后续的Item通过直接复用和间接复用完成。**
![enter description here][1]

## 优化
### ConvertView  
主要用于缓存view，如果为Null需要inflate创建新的view，如果不为null可以复用。也就是刚刚从屏幕消失的Item的view会被新划入页面的item所复用。
### ViewHolder
在实现Adapter的时候，我们一般会加上ViewHolder这个东西，ViewHolder和复用机制和原理是无关的，他的主要目的是持有Item中控件的引用，从而减少findViewById()的次数，因为findViewById()方法也是会影响效率的，因此在复用的时候他起的作用是减少方法执行次数增加效率。
### 滑动
图片加载，可以设置滑动监听事件，滑动结束开始加载，滑动开始停止加载。避免半透明元素，开启硬件加速。

参考：https://www.cnblogs.com/RGogoing/p/5554086.html


  [1]: ./images/Image 1.png