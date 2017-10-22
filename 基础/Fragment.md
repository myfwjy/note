---
title: Fragment
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 第五大组件
### 为什么被称为第五大组件
有生命周期但依附activity，大界面灵活应用UI，比activity更节省内存
### 加载到activity的两种方式
静态加载：加到布局里面
动态加载：在activity中添加，获取manager，通过管理器创建transaction，用transaction向指定id容器添加fragement，最后提交。
### FragmentPageAdapter和FragmentStatePageAdapter的区别
viewpager是支持页面滑动的组件，fragment负责显示页面。两个adapter，前者更加消耗内存，因为前者在滑动时只是断开了UI与fragment的连接，后者适合多页面，因为后者在destoryItem（切换页面）时，真正释放的内存。
## 生命周期
## 通信

 1. 在Fragment中调用Activity的方法 getActivity
 2. 在Activity中调用Fragment的方法 接口回调
 3. 在Fragment中调用Fragment的方法 先找到对应Activity再findFragmentById找到对应的Fragment

## 管理器：FragmentManager
replace：替换
add：添加
remove：移除