---
title: Spring Boot 笔记
tags: Spring Boot
grammar_cjkRuby: true
---

## 启动方式
1、直接点击IdeaIU中的Run按钮
2、cmd cd到项目文件夹下面，然后输入`mvn spring-boot:run`
3、cmd cd到项目文件夹下面，然后输入`mvn install`，cd target目录下，`java -jar girl-0.0.1-SNAPSHOT.jar`（可以通过dir查看target目录，选择自己项目的.jar文件）

## 使用不同的配置文件
![enter description here][1]


  [1]: ./images/QQ%E6%88%AA%E5%9B%BE20180225204349.png "QQ截图20180225204349"
  -dev是开发时配置文件，-prod是生产时配置文件
  在application.yml中配置
  

``` yml
spring:
  profiles:
    active: dev
```
那么正常默认启动是使用application-dev文件，

``` yml
server:
  port: 8080
girl:
  name: sherry
  age: 18
```
如果想双启动，可以cmd cd到项目文件夹下面，然后输入`mvn install`，cd target目录下，`java -jar girl-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod`
那么服务端也会启动application-prod文件的内容

``` yml
server:
  port: 8081
girl:
  name: sherry1
  age: 18
```
浏览器输入http://127.0.0.1:8081/hello或者http://127.0.0.1:8080/hello 能获取到不同的值。