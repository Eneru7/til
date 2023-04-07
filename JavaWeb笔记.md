【IDEA配置Tomcat】

在IDEA中部署配置Tomcat时，有两个地址相关的参数：

一个是Server下的URL

一个是Deployment下的Application Context

【URL】没有什么特殊，无所谓写什么，不写也可以，它只是启动TomcatServer后IDEA默认打开的一个地址。正确填写对开发人员更便利，什么是正确填写？继续往下看。

【Application Context】真正决定了Web项目的访问地址。

以部署在本地的TomcatServer举例，假设Web项目部署在8080端口，

如果Application Context设置为"/"，那么Web项目的主页就是http://localhost:8080。后续的访问都以此为基础。

如果Application Context设置为"/web"，那么Web项目的主页就是http://localhost:8080/web。后续的访问都以此为基础。

【一定要以"/"开头吗？】不是。看下面举例：

设置为"/aaa"，访问主页地址是http://locaohost:8080/aaa

设置为"aaa"，访问主页地址也是http://locaohost:8080/aaa

因此，如果Application Context设置为""（空），访问主页地址也是http://locaohost:8080

一般而言以"/"开头可以让路径更加清晰，因为有时会设置层级更深的路径，

比如如"/xxx/yyy/zzz"，那么访问主页的地址就是http://locaohost:8080/xxx/yyy/zzz

【启动TomcatServer】添加tomcat依赖

注意不同Tomcat版本，依赖名字不一样，详细见官网

【创建Servlet的两种方法】

1.@WebServlet("xx")

2.web.xml

以下情况会出现错误，A类用@WebServlet("/a")标注，web.xml中出现为"/a"的url-pattern时会报错，出现为A类全类名的servlet-class也会报错。

注意这里出现的url必须以"/"开始。

【以实现Servlet接口为例，研究Servlet的生命周期】

首次请求找到对应Servlet后，构造->init->service

后面多次访问都是执行service方法

关闭TomcatServer后，执行destroy

html中的form标签中的action属性，决定了跳转的位置

其中有无"/"是不一样的

假设项目地址:http://localhost:8080/web

action="/login"时，访问地址http://localhost:8080/login

action="login"时，访问地址http://localhost:8080/web/login



重定向和请求转发

重定向状态码302，可以重定向内部的地址，也可以是外部的url

resp.sendRedirect("xxx");【字符串参数以"/"开头会从根目录开始】

请求转发是服务器内部的跳转机制，目的是将本次请求转发给其他servlet处理

resp.getRequestDispatcher("/xxx").forward(req.resp);

请求转发不能跨域