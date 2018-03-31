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


  [1]: ./images/871676-20160722213407794-1894786938.png "871676-20160722213407794-1894786938"
  用户登录时提供用户名和密码，包装成token令牌交给Security Manager管理，shiro不能知道该用户是否合法，所以要交给Realm，由设计者自行判断，在Spot中使用的是mysql数据库存储。