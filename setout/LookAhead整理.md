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
代码中，先拿到证书和证书密码和apns建立连接，然后创建payload，里面带有需要发送的信息，还有图标小红圈、铃音之类的，最后带着devicetoken推送出去。
### scheduled-tasks
定时任务调度，设定时间自动进行调度。有两种方式，一种是在xml中设置，一种是使用注解的方式。本项目用的是xml文件配置，首先声明一个job，需要给一个spring注解@service或者@component，然后在xml中设置他的启动方法，启动时间，启动间隔。
### 与第三方交互获取行程最优路径
根据第三方提供的api式样书方案，发送请求。
###  更新计划行程拥堵信息
定时去数据库中获取需要更新的行程，然后包装发送给docomo api获取组成行程的坐标集，做成csv文件上传给s3，通过apns推送给用户更新信息，存储csv文件地址，客户端之后可以通过地址去s3上获取行程计划。
### 冗余信息是什么
定期清理无效的devicetoken。
### mybatis配置使用的整体过程
mybatis是一款持久层框架，支持定制sql、存储过程和高级映射。
声明jdbc.properties文件，设置数据库源信息。
在上下文配置文件.xml中配置DB数据源，添加SqlSessionFactoryBean的配置，添加SqlSessionTemplate的配置。
在dao层中注入SqlSessionTemplate，使用他的selectone或者selectlist方法，传入类全名和方法名。


