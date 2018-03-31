---
title: Spot检索整理
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### MVC MVP MVVM
mvc：view传递指令到controller，controller完成业务逻辑后要求model修改状态，model将新的数据反馈给用户view。
mvp(presenter)：双向通信，view和model不发生联系，都通过presenter传递。
mvvm(view model)：view和viewmodel双向绑定，view有变动自动反馈到viewmodel。

### Restful Api
Restful框架:Representational State Transfer。网络上的资源（URI），客户端与服务端之间传递资源的某种表面层（文本可能是txt，也可能是html）,通过无状态的http的四个动作（get post update delete），对服务器资源进行操作，实现表现层状态转化。
为了应对多样的前端与后端进行通信，需要有统一的机制，Restful Api是一种互联网应用程序API的设计理论。比如路径中不要有动词，用http动词代替。

### Cookie Session
Cookie保存在客户端，是服务端发给客户端的特殊信息，存放在HTTP请求头（Request Header），由于http是一种无状态协议，需要cookie辅助，在下一次发送请求时带上用户信息，服务端就可以动态分析请求头，生成与客户端对应的信息。比如使用浏览器登录时的记住密码。
Session是服务端使用的记录客户端状态的机制，运行过程中生成session，返回给客户端session id，下一次请求时会根据客户端传来的id找到对应的session,获取再服务端保存的用户状态。
### Shiro
![enter description here][1]


用户登录时提供用户名和密码，包装成token令牌交给Security Manager管理，shiro不能知道该用户是否合法，所以要交给Realm，由设计者自行判断，在Spot中使用的是mysql数据库存储。
过滤器filter是通过回调函数使用，interceptor拦截器是通过代理，aop使用。
shiro filter中的关键字anon是指没有参数，可以匿名使用，auth表示必须登录才能使用。注意路径匹配顺序，匹配到以后就不会继续匹配，所有通配符的要放在后面。
![enter description here][2]


### Spring MVC
#### web.xml
![enter description here][3]
DispatcherServlet（前端控制器），来自前端的请求会先到达这里，它负责到后台去匹配合适的handler，在调用对应的controller、service、dao。使用serlvet需要对每一个请求配置对应的节点，DispatcherServlet会拦截所有的请求，然后去查找有没有合适的处理器。
![enter description here][4]
DispatcherServlet拦截哪些url进行处理。
![enter description here][5]
ContextConfigLocation：配置上下文位置。
ContextLoaderListener：在启动Web容器时，自动装配Spring applicationContext.xml的配置信息。
ShiroFilter：拦截器部分交给shiro。
CharacterEncodingFilter：同意编码过滤器，在httpServletRequest到达serlvet时，在httpServletResponse到达客户端之前修改头和数据。
#### SpringServlet-config.xml
![enter description here][6]
视图解析器，controller返回的都是逻辑视图，会交给视图解析器添加开头结尾。

  [1]: ./images/871676-20160722213407794-1894786938.png "871676-20160722213407794-1894786938"
  [2]: ./images/QQ%E6%88%AA%E5%9B%BE20180331201954.png "QQ截图20180331201954"
  [3]: ./images/QQ%E6%88%AA%E5%9B%BE20180331204602.png "QQ截图20180331204602"
  [4]: ./images/QQ%E6%88%AA%E5%9B%BE20180331205801.png "QQ截图20180331205801"
  [5]: ./images/QQ%E6%88%AA%E5%9B%BE20180331210130.png "QQ截图20180331210130"
  [6]: ./images/QQ%E6%88%AA%E5%9B%BE20180331211517.png "QQ截图20180331211517"