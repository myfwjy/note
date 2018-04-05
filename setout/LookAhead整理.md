---
title: LookAhead整理
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### APNS
APNs(Apple Push Notification service)是远程推送功能的核心，通过APNs客户端和苹果服务器建立一个长连接，推送也是通过这个长连接发送到客户端上。
deviceToken是设备的一个标识符，属于你这款APP装在你这个设备上的标识符，即每个APP在每一个不同的设备上都有着不同的deviceToekn，通过注册远程推送服务，APNs会返回给你的APP的deviceToken，如图
![enter description here](http://7xsnb0.com1.z0.glb.clouddn.com/2016-07-29_09:22:33.jpg)

为了保证安全性，APNs用连接信任(connection trust)和token信任(token trust)来控制通信入口，你要用APNs则必须用通过这两种验证。
连接信任是provider必须有开发者官网申请申请的证书，token信任是每次发送给苹果服务器的信息要带token。