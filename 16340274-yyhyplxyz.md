# 技术报告

16340274 杨元昊

​	本次我们小组使用flask框架来完成作业后端，flask是python最常用的框架之一，它具有小巧灵活，定制度高的特点。

​	从业务逻辑上而言，这个程序并不复杂，缺少的是细心和耐心。值得一提的是本次作业我更灵活的使用了python异步编程和装饰器的相关知识。同时对flask的相关原理有了更清晰的认识。

​	在发送邮件的相关模块中，我通过开启多个线程进行邮件发送的操作，避免了IO阻塞。

​	在蓝图注册的过程中，我利用装饰器的知识新建了属于自己的蓝图，用以区分v1，v2版本的api，更好地实现了可扩展性。同时，我也用装饰器保证了数据库提交发生错误的时候发生回滚。我写了相关博客，可以参阅。http://yangyuanhao.com/2019/06/30/Python%E4%B8%AD%E7%9A%84%E4%BF%AE%E9%A5%B0%E5%99%A8%E6%A8%A1%E5%BC%8F/

​	以下将简单阐述下使用flask的心得体会。

## 应用上下文与请求上下文

Flask提供了两种上下文，一种是应用上下文(Application Context)，一种是请求上下文(Request Context)。
 可以查看Flask的文档：[应用上下文](https://link.jianshu.com?t=http://flask.pocoo.org/docs/0.11/appcontext/)   [请求上下文](https://link.jianshu.com?t=http://flask.pocoo.org/docs/0.11/reqcontext/)

通俗地解释一下**application context**与**request context**：

> 1.  *application* 指的就是当你调用app = Flask(**name**)创建的这个对象app；

1.  *request* 指的是每次http请求发生时，WSGI server(比如gunicorn)调Flask.**call**()之后，在Flask对象内部创建的Request对象；
2.  *application* 表示用于响应WSGI请求的应用本身，*request* 表示每次http请求;
3.  *application*的生命周期大于*request*，一个*application*存活期间，可能发生多次http请求，所以，也就会有多个*request*

为了在一个线程中更加方便使用这些变量，flask中还有一种堆栈的数据结构（通过werkzeug的LocalStack实现），可以处理这些变量，但是并不直接处理这些变量。假如有一个程序得到一个请求，那么flask会将这个请求的所有相关信息进行打包，打包形成的东西就是处理请求的一个环境。flask将这种环境称为“请求上下文”(request context)，之后flask会将这个请求上下文对象放到堆栈中。这样，请求发生时，我们一般都会指向堆栈中的“请求上下文”对象，这样可以通过请求上下文获取相关对象并直接访问，例如current_app、request、session、g。还可以通过调用对象的方法或者属性获取其他信息，例如request.method。等请求结束后，请求上下文会被销毁，堆栈重新等待新的请求上下文对象被放入。

当在一个应用的请求上下文环境中，需要嵌套处理另一个应用的相关操作时（这种情况更多的是用于测试或者在console中对多个应用进行相关处理），“请求上下文”显然就不能很好地解决问题了，Flask中将应用相关的信息单独拿出来，形成一个“应用上下文”对象。这个对象可以和“请求上下文”一起使用，也可以单独拿出来使用。不过有一点需要注意的是：在创建“请求上下文”时一定要创建一个“应用上下文”对象。比如app = Flask(**name**)构造出一个 Flask App 时，App Context 并不会被自动推入 Stack 中。所以此时 Local Stack 的栈顶是空的，current_app也是 unbound 状态，在编写离线脚本的时候，如果直接在一个 Flask-SQLAlchemy 写成的 Model 上调用 User.query.get(user_id)，就会遇到 RuntimeError。因为此时 App Context 还没被推入栈中，而 Flask-SQLAlchemy 需要数据库连接信息时就会去取 current_app.config，current_app 指向的却是 _app_ctx_stack为空的栈顶。

因此运行脚本正文之前，先将 App 的 App Context 推入栈中，栈顶不为空后 current_app这个 Local Proxy 对象就自然能将“取 config 属性” 的动作转发到当前 App 上。

## WSGI

Python有着许多的Web框架，而同时又有着许多的Web 服务器（除了Nginx以外，还有Apache等），Web框架和Web服务器之间需要进行通信，如果在设计时它们之间不可以相互匹配的，那么选择了一个框架就会限制对Web服务器的选择，这显然是不合理的。

怎样确保可以在不修改Web服务器代码或Web框架代码的前提下，使用自己选择的服务器，并且匹配多个不同的网络框架呢？答案是接口，设计一套双方都遵守的接口就可以了。对Python来说，就是**WSGI**（Web Server Gateway Interface，Web服务器网关接口）。其他编程语言也拥有类似的接口：例如Java的Servlet API和Ruby的Rack。

Python WSGI的出现，让开发者可以将Web框架与Web服务器的选择分隔开来，不再相互限制。现在，你可以真正地将不同的Web服务器与Web框架进行混合搭配，选择满足自己需求的组合。



