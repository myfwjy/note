---
title: Spring面试题整理
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### aop
面向切面编程，代码运行时动态插入代码，降低耦合度，能添加辅助功能，参考logger和事物。
### IOC
参考IOC的文档
### 模式
工厂：生成相似的对象，对象的new方法又比较复杂，比如线程池，fixThread、singleThread，前几位参数相同。
单例：一个类只有一个对象。bean默认是singleton。
代理：通过代理对象访问目标对象。比如mybatis就是使用的jdk内的动态代理，只需要写接口，动态在内存中生成代理对象。
https://www.cnblogs.com/cenyu/p/6289209.html
观察者：在对象之间定义了一对多的依赖，这样一来，当一个对象改变状态，依赖它的对象会收到通知并自动更新。
### 事务管理
代码中使用的是声明式事务，在xml中配置，在代码中使用标签。
### spring流程
1. 用户发送请求到dispatcherSerlvet
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet通过HandlerAdapter处理器适配器调用处理器
5. 执行处理器(Controller，也叫后端控制器)。
6. Controller执行完成返回ModelAndView
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。
11. DispatcherServlet响应用户
### BeanFactory 接口和 ApplicationContext 接口有什么区别 ？
ApplicationContext继承BeanFactory接口，Spring的核心工厂就是BeanFactory，ApplicationContext是会在加载配置文件时初始化Bean。ApplicationContext可以进行国际化的对应，bean的自动装配等。