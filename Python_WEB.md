# WEB

## 1. 简单介绍一下Django框架

Django是一个开源的Web框架，使用Python编写的，采用的是MTV的设计模式。使用Django框架可以让程序员以最小的代价构建和维护高质量的Web应用

## 2. 如何处理单点登录问题

## 3. 在Python的Django中如何进行事物处理

首先在创建的事物中添加一个@transaction.atomic()的装饰器，这部分需要先引入一个叫 from django.db import transction。然后根据实际的业务逻辑进行相应的处理。在进行事物处理的时候需要注意的事如果在处理过程中只要有一步出现异常，那么就说明所有的事物都要rollback进行事物回退。

## 4. 简述Django的ORM

ORM全程Object-Relation Mapping意味对象关系映射

DJango的ORM系统体现在框架内就是模型层，关键在于用代码的方式来定义数据库表的做法。ORM将python对象映射成数据库中的一张关系表。即一个python类，就是一个模型，代表数据库中的一张表。它将SQL封装起来，程序员不再关心数据库的具体操作，只需要专注于python代码和业务逻辑的实现即可。它的实现过程就是:python代码--通过ORM转换成SQL语句，再通过pymysql库去实际操作数据库。每个模型都是DJango.db.models.Model的子类，每个类的属性代表数据库表和属性名同名的字段。

## 5. python中的get请求和post请求有何区别

- **在request库中**: 
  - 1、在客户端，get方式在通过url提交数据，数据在url中可以看到，post方式，数据放在HTML Header中
  - 2、get方式提交的数据最多只有1024byte，而post没有限制
  - 3、在安全性上，如果使用get方式，则参数会在地址栏上显示，而post由于数据放在http包的包体中，所以在地址栏上不会显示。在实际问题中如果是中文数据，而且是非敏感数据，可以使用get；如果用户输入的数据不是中文而且包含敏感数据，那么最好使用post为好。
- **在表单提交中**:
  - 1、get是从服务器上获取数据，post是像服务器发送数据
  - 2、对于表单提交的方式，在服务器端只能用Request.QueryString来获取get方式提交的数据，用post方式提交的数据智能用Request.Form来获取。
  - 3、一般来说，应尽量避免使用get方式提交数据，因为有可能会导致安全性问题。比如在登录表单中使用get方式，用户输入的用户民和密码在地址栏中显示。而在分页中，使用get比使用post好。
- **在HTTP中**:
  - 1、post是被设计用来向上放东西的，而get是被设计来从服务器中获取东西的。get也能够向服务器传送少量的数据，而get之所以也能传数据，展示用来告诉服务器需要什么样的数据。post的信息作为HTTP请求的内容，而get是在HTTP头部传输的
  - 2、POST和get的传输方式不同，get的参数是在HTTP的头部传送，而post的数据则是显示在HTTP请求的请求内容中
  - 3、get方法由于受到url长度的限制，只能最大传递1024字节，post传输的数据量大，可以达到2M，而根据微软方面的说法，微软对用Request.form()字节，IIS5中为100字节。

## 7. 分别从前端、后端、数据库阐述web项目性能优化

- **前端优化**: 
  - 1、减少http请求，例如制作精灵图
  - 2、html和css放在页面上部，JavaScript放在页面下部，以为js加载比html和css加载慢，所以要优先。
- **后端优化**:
  - 1、缓存存储读写次数高，变化少的数据，比如网站首页的信息、商品额信息等。
  - 2、异步方式，如果有耗时操作，可以采用异步，比如celery
  - 3、代码优化，避免循环和判断次数太多，可如果多个if else判断，优先判断最有可能发生的情况
- **数据库优化**:
  - 1、如有条件，数据可以存放Redis，读取速度快
  - 2、建立索引，外键等

## 8. Django中当一个用户登录A应用服务器(进入登录状态),然后下次请求被nginx代理到B应用会出现什么影响

如果用户在A应用服务器登录的session数据没有共享到B的话，那么之前的登录状态就没有了

## 9. 跨域请求问题Django怎么解决(解决原理)

启用中间件

post请求

验证码

表单中添加(% csrf_token %)标签

## 10. 请求解释或描述一个Django的架构

**Django框架遵循MVC设计，并且有一个专有名词: MTV **

- **M**: Model，与MVC中的M功能相同，负责数据处理，内嵌了ORM框架
- **V**：View，与MVC中的C相同，接收HttpRequest，业务处理，返回HTTPResponse
- **T**: Template，与MVC种的V功能相同，负责封装构造要返回的html，内嵌了模板引擎

## 11. Django对数据查询结果排序怎么做，降序怎么做，查询大于某个字段怎么做

排序使用order_by()

降序需要在排序字段名前加"-"

查询大于某个值，使用filter(字段名_gt=值)

## 12. Django，MIDDLEWARES中间件的作用

中间件是介于request和response之间的一道处理过程，相对比价轻量级，并且在全局上改变Django的输入域输出

## 13. 对Django的认识

- Django是走大而全的方向，它最出名的事其全自动化后台管理: 只需要使用ORM，坐简单地对象定义，它就能自动生成数据库结构，以及全功能的管理后台。
- Django内置的ORM跟框架内的其它模块耦合程度很高。
- 应用程序必须使用Django内置的ORM，否者就不能享受到框架提供的种种基于ORM的便利；理论上可以切换掉ORM模块，但这就相当于把装修完的房子拆除重建。
- DJango的特点是超高的开发效率，其性能扩展有限；采用DJango的项目，在流量达到一定规模后，都需要对其进行重构，才能满足性能需求。
- Django使用的是中小型网站，或者是作为大型网站快速实现产品雏形的工具。
- DJango模板的设计哲学是彻底的将代码、样式分离；DJango从根本上杜绝在模板中进行编码，处理数据的可能。

## 14. DJango重定向是如何实现的。用什么状态码

使用HttpResponseRedirect

redirect和reverse

状态码: 302, 301

## 15. Tornado的核心是什么

Tornado的核心是ioloop和iostream这两个模块，前者提供了一个高效的I/O事件循环，后者则封装了一个无阻塞的socket。通过向ioloop中添加网咯I/O事件，利用无阻塞的socket，再搭配相应的回调函数，便可达到高效的异步执行。

## 16. nginx的正向代理和反向代理

- **正向代理**: 是一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端代理发送一个请求并制定目标(原始服务器) ，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理
- **反向代理**: 反向代理和正向代理正好相反，对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容原本就是它自己的一样

## 17. DJango本身提供了runserver，为什么不用来部署

runserver方法是调试DJango时经常用到的方式，它使用DJango自带的WSGI Server运行，主要在测试和开发中使用，并且runserver开启的方式也是单进程。UWSGI是一个Weg服务器，它实现了WSGI协议、uwsgi、http等协议。uwsgi是一种通信协议，而uWSGI是实现uwsggi协议和WSGI协议的Web服务器。uWSGI具有超快的性能，低内存占用和多app管理等优点，并搭配这Nginx。

## 18. DJango如何处理请求

- **1、** 当用户请求一个页面时，DJango会根据用户输入的请求决定使用哪个URLconf模块。通常，是ROOT_URLCONF设置的值，默认是项目文件下的根路由。但是如果传入的事HttpRequest对象具有urlconf属性(由中间件设置)，则其值将被用于代理ROOT_ULRCONF设置。通俗的讲，就是可以自定义项目入口url是哪个文件
- **2、**加载该模块并寻找可用的urlpatterns。它是django.conf.urls.url()实例的一个列表
- 3、一次匹配每个URL模式，在于请求的URL相匹配的第一个模式停下来。也就是说，url匹配时从上往下的短路操作，所以url在列表中的位置非常关键
- **4、**导入并调用匹配行中固定的视图，该视图是一个简单的python函数(被称为视图函数)，或基于类的视图。视图将后的如下参数:
  - 1. 一个HttpRequest实例
    2. 如果匹配的正则表达式返回了没有命名的组，那么正则表达式匹配的内容将作为参数提供给试图
    3. 关键字参数由正则表达式匹配的命名组组成，但是可以被django.conf.urls.url()的可选参数kwargs覆盖
- 如果没有匹配到正则表达式，或者过程中抛出异常，将调用一个适当的错误处理试图

## 19. DJango如何定义url请求的错误页面

**当DJango找不到于请求匹配的URL时，或者当抛出一个异常时，将调用一个错误处理试图。错误试图包括400、403、404和500，分别表示请求错误、拒绝服务、页面不存在和服务器错误。**

**它们分别位于**:

- handler400----django.conf.urls.handler400
- handler403----django.conf.urls.handler403
- handler404----django.conf.urls.handler404
- handler500----django.conf.urls.handler500

**这些值可以在ULRconf中设置。在其他app中的二级URLconf中设置这些变量无效**

## 20. 简述你对python WSGI的理解

## 21. DJango如何处理文件上传

DJango在处理文件上传时，文件数据被打包封装在request.FILES 中。处理这个表单的视图将在request.FILES中收到文件数据，可以用request.FILES['file']来获取上传文件的具体数据。其中的键值'file'是根据file=forms.FileField()变量名来的。request.FILES只有在请求方法为POST，并且提交请求的<form>表单具有enctype='multipart/form-data'属性时才有效，否者request.FILES将为空

## 22. DJango如何实现对会话的支持

DJango的session框架支持匿名会话，封装了cookies的发送和接收过程。cookie包含一个会话ID而不是数据本身。DJango的会话框完全地、唯一地基于cookie。DJango通过一个内置中间件来实现会话功能。要启用会话就要先启动中间件。编辑settings.py中的MIDDLEWARE设置，确保存在于django.contrib.sessions.middleware.SessionMiddleware这行。默认情况下在新建项目中是存在的。并且默认情况下，DJango将会话数据保存在数据库内。但是，需要确保在INSTALLED_APPS中设置django.contrib.sessions存在，然后运行manage.py。

## 23. DJango会话的生存周期

默认情况下，SESSION_EXPIRE_AT_BROWSER_CLOSE设置为False，也就是说cookie保存在用户浏览器内，直到失效日期，这样用户就不必每次打开浏览器后都要登录一次，相反的SESSION_EXPIRE_AT_BROWSER_CLOSE设置为True，则以为着浏览器一关闭，cookie失效，每次重新打开浏览器，就得重新登陆。

## 24. DJango如何清除已保存的会话

随着用户的访问，会话数据越来越大。如果使用的是数据库保存模式，那么DJango_session表的内容会逐渐增加。可如果使用的是文件模式，那么临时目录内的文件数量会不断增加。造成这个问题的原因是，如果用户手动提出登录，DJango将会自动删除会话数据，但是如果用户不退出登录，那么对应的会话数据不会删除。DJango没有提供自动秦楚失效会话的机制，需要程序员自己完成这部分内容。但是，DJango提供了一个命令，clearsession用于清除会话数据，可以根据这个命令设置一个周期性的自动清除机制，比如crontab或者Windows调度任务。但是如果使用的是缓存模式的话是不需要清理数据，因为缓存系统自己有清理过期数据的机制。使用cookie模式的会话也不需要，因为数据都存在用户的浏览器内，不需要手动清理。

## 25. 简单描述一下Django的网站地图(sitemap)

Django的网站梯度是根据网站的结构、框架、内容生成的导航页，是一个网站所有连接器。很多网站的链接层次比较深，蜘蛛很难抓到，网站地图可以方便搜索引擎或者网络爬虫抓取网站页面，了解网站结构，为网络爬虫指路，增加网站内容页面的收录概率。网站地图一般存放在域名根目录下并命名为sitemap。DJang自带了一个高级的生成网站地图的框架，可以很容易地创建出XML格式的网站地图。创建网站地图，只需要编写一个sitemap类，并在URLconf中编写对应的访问路由。

## 26. 简单描述一下Django的信号机制(singal)

Django自带的一套信号机制来帮助我们在框架的不同位置之间传递消息。也就是说，的当某一事件发生时，信号系统可以允许一个或多个发送者(senders)将通知或信号(singals)发送给一个接收者(receivers)。信号系统包括三个要求:

- 1、发送者----信号的发送方
- 2、信号----信号本身
- 3、接受者----信号的接受者

## 27. 怎样理解Django的序列化

Django序列化是通过其强大的序列化工具serializers完成的，它可以将DJango的模型“翻译”成其他格式的数据。通常情况下，这种其它格式的数据是基于文本的，并且用于数据交换\传输过程。首先，需要从django.core导入它，然后调用它的serialize方法，这个方法至少接收两个参数，第一个是要序列化成韦德数据格式，第二个是要序列化的数据对象，数据通常时ORM模型的queryset，一个可迭代的对象。

## 28. 如果在网页中，当处理完表单中用户输入的信息后，如和将通知发送给客户

django提供了基于cookie或者会话的消息框架message，无论是匿名用户还是认证用户，这个消息框架允许你临时将消息存储在请求中，并在接下来的请求中提取他们并显示。每个消息都带有一个特定的level标签，表示优先级。django的message消息框架的实现，依赖于message中间件和对应的context processor。通过django-admin startproject。命令创建工程的时候，已经默认在settings.py中开启了消息框架功能需要的所有位置。并且已经有默认的配置，一般使用默认的配置即足够使用了。

## 29. django模板的加载顺序

- 1、先去配置模板目录下面寻找模板文件
- 2、去INSTALL_APPS下面的每个应用中去找模板文件，前提是应用中必须有template文件夹

## 30. django模板变量的解析顺序

模板变量名由数字，字母，下划线和点组成，不能由下划线开头，使用模板变量时(以{{book.title 为例}})，首先会把book当成一个字典，把title当成键名，进行取值book['title']，如果没有取到，则会把cook当成一个对象，把title当成属性，进行取值，book.title。如果还没取到，则把book当成对象，把title当成对象的方法，进行取值book.title。如果都没有取道，则会返回空字符串填充模板变量。

## 31. django中间件的执行流程

当浏览器发送请求给服务器的时候会产生一个request对象，这时会调用中间件类中的process-request对象，然后djangohi进行url配置，之后会调用中间件类中的process_view。接着会调用视图函数，最后会调用中间件类中的process_response，最终将结果返回给浏览器。

## 32. 如何理解http应答码

**http应答码也就是状态码，它反应了Web服务器处理HTTP请求的状态。HTTP应答码由3位数字构成，其中首位数字定义了应答码的类型**

- **1xx**: 信息类，表示接收到Web浏览器请求，正在进一步处理中
- **2xx**: 成功类，表示用户请求被正确接受
- **3xx**:重定向类，表示请求没有成功，客户必须采取进一步动作
- **4xx**: 客户端错误，表示客户端提交的请求有错误
- **5xx**:服务器错误，表示服务器不能完成对请求的处理

## 33. cookie可能引起的安全问题

## 34. django rest form架构

## 40. REST(Representational State Transfer)

REST 是具象状态传输或者表现层状态，是一种软件架构风格，而不是标准。它的要点是资源由URL来指定。通过资源的表现形式来操纵资源，对资源的操作宝库欧获取、创建、修改和删除资源，对应的HTTP协议提供GET、POST、PUT、DELETE方法。