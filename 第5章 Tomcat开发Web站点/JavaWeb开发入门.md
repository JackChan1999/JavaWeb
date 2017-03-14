# **1. Web概述**

WEB，在英语中web即表示网页的意思，它用于表示Internet主机上供外界访问的资源。javaweb：使用java技术开发web页面。供浏览器访问的项目

Internet上供外界访问的Web资源分为：

- 静态web资源（如html 页面）：指web页面中供人们浏览的数据始终是不变的，静态的，不同的人在不同的时间来访问时都是相同的内容
- 动态web资源：指web页面中供人们浏览的数据是由程序产生的，不同时间点访问web页面看到的内容各不相同。

静态web资源开发技术：Html、CSS、javaScript

常用动态web资源开发技术：

- JSP/Servlet、ASP、PHP等
- 在Java中，动态web资源开发技术统称为Javaweb，我们课程的重点也是教大家如何使用Java技术开发动态的web资源，即动态web页面。

![静态网页与动态网页](http://img.blog.csdn.net/20161030163430160)

# **2. WEB服务器**

Web服务器的作用是接收客户端的请求，给客户端作出响应。学习web开发，需要先安装一台web服务器，然后再在web服务器中开发相应的web资源，供用户使用浏览器访问。

注意：这里所说的服务器不是指服务器硬件资源，而是指服务器软件。

实验说明：

- 在本地计算机上随便创建一个web页面，大家可以访问到吗？
- 启动tomcat服务器，把web页面放在tomcat服务器中，用户就可以访问了。

这说明什么问题？

- 不管什么web资源，想被远程计算机访问，都必须有一个与之对应的网络通信程序，当用户来访问时，这个网络通信程序读取web资源数据，并把数据发送给来访者。
- WEB服务器就是这样一个程序，它用于完成底层网络通迅。使用这些服务器，用户只需要关注web资源怎么编写，而不需要关心资源如何发送到客户端手中，从而极大的减轻了开发者的开发工作量。

# **3. 常见WEB服务器**

- Tomcat（Apache）：当前应用最广的JavaWeb服务器
- JBoss（Redhat红帽）：支持JavaEE，应用比较广；EJB容器
- GlassFish（Orcale）：Oracle开发JavaWeb服务器，应用不是很广
- Resin（Caucho）：支持JavaEE，应用越来越广
- Weblogic（Orcale）：要钱的！支持JavaEE，适合大型项目
- Websphere（IBM）：要钱的！支持JavaEE，适合大型项目

## **3.1 WebLogic**

WebLogic是BEA公司的产品，是目前应用最广泛的Web服务器，支持J2EE规范，而且不断的完善以适应新的开发要求，启动界面如图

![WebLogic](http://img.blog.csdn.net/20161030124155383)

## **3.2 WebSphere**

另一个常用的Web服务器是IBM公司的WebSphere，支持J2EE规范，启动界面如图

![WebSphere](http://img.blog.csdn.net/20161030124213186)

## **3.3 Tomcat**

在小型的应用系统或者有特殊需要的系统中，可以使用一个免费的Web服务器：Tomcat，该服务器支持全部JSP以及Servlet规范，启动界面如图

![Tomcat](http://img.blog.csdn.net/20161030124244702)

Tomcat服务器由Apache提供，开源免费。由于Sun和其他公司参与到了Tomcat的开发中，所以最新的JSP/Servlet规范总是能在Tomcat中体现出来。当前最新版本是Tomcat8，我们课程中使用Tomcat7。Tomcat7支持Servlet3.0，而Tomcat6只支持Servlet2.5！

# **4. JavaEE概述**

java的大方向就是JavaEE，JavaEE不仅仅是socket编程，具体包括13中核心技术。

JAVAEE的核心API与组件

JAVAEE平台由一整套服务（Services）、应用程序接口（APIs）和协议构成，它对开发基于Web的多层应用提供了功能支持，下面对JAVAEE中的13种技术规范进行简单的描述(限于篇幅，这里只进行简单的描述)：

## **4.1 JDBC(Java Database Connectivity)** 　　

JDBC API为访问不同的数据库提供了一种统一的途径，象ODBC一样，JDBC对开发者屏蔽了一些细节问题，另外，JDCB对数据库的访问也具有平台无关性。

## **4.2 JNDI(Java Name and Directory Interface)**

JNDI API被用于执行名字和目录服务。它提供了一致的模型来存取和操作企业级的资源如DNS和LDAP，本地文件系统，或应用服务器中的对象。

## **4.3 EJB(Enterprise JavaBean)** 　　

JAVAEE技术之所以赢得媒体广泛重视的原因之一就是EJB。它们提供了一个框架来开发和实施分布式商务逻辑，由此很显著地简化了具有可伸缩性和高度复杂的企业级应用的开发。EJB规范定义了EJB组件在何时如何与它们的容器进行交互作用。容器负责提供公用的服务，例如目录服务、事务管理、安全性、资源缓冲池以及容错性。但这里值得注意的是，EJB并不是实现JAVAEE的唯一途径。正是由于JAVAEE的开放性，使得有的厂商能够以一种和EJB平行的方式来达到同样的目的。

## **4.4 RMI(Remote Method Invoke)** 　　

正如其名字所表示的那样，RMI协议调用远程对象上方法。它使用了序列化方式在客户端和服务器端传递数据。RMI是一种被EJB使用的更底层的协议。

## **4.5 Java IDL/CORBA** 　　

在Java IDL的支持下，开发人员可以将Java和CORBA集成在一起。他们可以创建Java对象并使之可在CORBA ORB中展开, 或者他们还可以创建Java类并作为和其它ORB一起展开的CORBA对象的客户。后一种方法提供了另外一种途径，通过它Java可以被用于将你的新的应用和旧的系统相集成。

## **4.6 JSP(Java Server Pages)** 　　

JSP页面由HTML代码和嵌入其中的Java代码所组成。服务器在页面被客户端所请求以后对这些Java代码进行处理，然后将生成的HTML页面返回给客户端的浏览器。

## **4.7Java Servlet** 　　

Servlet是一种小型的Java程序，它扩展了Web服务器的功能。作为一种服务器端的应用，当被请求时开始执行，这和CGI Perl脚本很相似。Servlet提供的功能大多与JSP类似，不过实现的方式不同。JSP通常是大多数HTML代码中嵌入少量的Java代码，而servlets全部由Java写成并且生成HTML。

## **4.8 XML(Extensible Markup Language)** 　　

XML是一种可以用来定义其它标记语言的语言。它被用来在不同的商务过程中共享数据。 XML的发展和Java是相互独立的，但是，它和Java具有的相同目标正是平台独立性。通过将Java和XML的组合，您可以得到一个完美的具有平台独立性的解决方案。

## **4.9 JMS(Java Message Service)** 　　

JMS是用于和面向消息的中间件相互通信的应用程序接口(API)。它既支持点对点的域，有支持发布/订阅(publish/subscribe)类型的域，并且提供对下列类型的支持：经认可的消息传递,事务型消息的传递，一致性消息和具有持久性的订阅者支持。JMS还提供了另 一种方式来对您的应用与旧的后台系统相集成。

## **4.10、JTA(Java Transaction Architecture)** 　　

JTA定义了一种标准的API，应用系统由此可以访问各种事务监控。

## **4.11、JTS(Java Transaction Service)** 　　

JTS是CORBA OTS事务监控的基本的实现。JTS规定了事务管理器的实现方式。该事务管理器是在高层支持Java Transaction API (JTA)规范，并且在较底层实现OMG OTS specification的Java映像。JTS事务管理器为应用服务器、资源管理器、独立的应用以及通信资源管理器提供了事务服务。

## **4.12 JavaMail** 　　

JavaMail是用于存取邮件服务器的API，它提供了一套邮件服务器的抽象类。不仅支持SMTP服务器，也支持IMAP服务器。

## **4.13 JAF(JavaBeans Activation Framework)**

JavaMail利用JAF来处理MIME编码的邮件附件。MIME的字节流可以被转换成Java对象，或者转换自Java对象。大多数应用都可以不需要直接使用JAF

# **5. Tomcat服务器**

## **5.1 Tomcat 的下载与安装**
下载地址：http://tomcat.apache.org/

安装目录不能包含中文和空格

JAVA_HOME环境变量指定Tomcat运行时所要用的jdk所在的位置，注意，配到目录就行了，不用指定到bin

端口占用问题：netstat -ano命令查看端口占用信息

Catalina_Home环境变量：startup.bat启动哪个tomcat由此环境变量指定，如果不配置则启动当前tomcat，推荐不要配置此环境变量

## **5.2 Tomcat 的目录层次结构**
![Tomcat](http://img.blog.csdn.net/20161030130705805)

- bin--存放tomcat启动关闭所用的批处理文件
- conf--tomcat的配置文件，最终要的是server.xml
- lib--tomcat运行所需jar包
- logs--tomcat运行时产生的日志文件
- temp--tomcat运行时使用的临时目录，不需要我们关注
- webapps--web应用所应存放的目录
- work--tomcat工作目录，后面学jsp用到

## **5.3 启动和关闭Tomcat**
在启动Tomcat之前，我们必须要配置环境变量：

- JAVA_HOME：必须先配置JAVA_HOME，因为Tomcat启动需要使用JDK
- CATALANA_HOME：如果是安装版，那么还需要配置这个变量，这个变量用来指定Tomcat的安装路径，例如：F:\apache-tomcat-7.0.42
- 启动：进入%CATALANA_HOME%\bin目录，找到startup.bat，双击即可
- 关闭：进入%CATALANA_HOME%\bin目录，找到shutdown.bat，双击即可

startup.bat会调用catalina.bat，而catalina.bat会调用setclasspath.bat，setclasspath.bat会使用JAVA_HOME环境变量，所以我们必须在启动Tomcat之前把JAVA_HOME配置正确。

启动问题：点击startup.bat后窗口一闪即消失：检查JAVA_HOME环境变量配置是否正确；

## **5.4 配置端口号**

打开%CATALANA_HOME%\conf\server.xml文件

![CATALANA_HOME](http://img.blog.csdn.net/20161030164202678)

http默认端口号为80，也就是说在URL中不给出端口号时就表示使用80端口。当然你也可以修改为其它端口号。

当把端口号修改为80后，在浏览器中只需要输入：http://localhost就可以访问Tomcat主页了


# **6. 虚似主机**

一个真实主机可以运行多个网站，对于浏览器来说访问这些网站感觉起来就像这些网站都运行在自己的独立主机中一样，所以，我们可以说这里的每一个网站都运行在一个虚拟主机上，一个网站就是一个虚拟主机

## **6.1 配置虚似主机**

如需在WEB服务器中配置一个网站，需使用Host元素进行配置

在server.xml中&lt;Engine>标签下配置&lt;Host>,其中name属性指定虚拟主机名，appBase指定虚拟主机所在的目录

只在servlet.xml中配置Hosts，还不能是其他人通过虚拟主机名访问网站，还需要在DNS服务器上注册一把，我们可以使用hosts文件模拟这个过程

默认虚拟主机：在配置多个虚拟主机的情况下，如果浏览器使用ip地址直接访问网站时，该使用哪个虚拟主机响应呢？可以在&lt;Engine>标签上设置defaultHost来指定
```
<Host name=”site1” appBase=”c:\app”></Host>
```
配置的主机(网站)要想被外部访问，必须在DNS服务器或windows系统中注册

由于浏览器访问地址时,需要将地址翻译成对应的ip才能找到服务器,这其中翻译的过程是由dns服务器来实现的.我们在做实验的时候没有办法去修改dns服务器,此时可以使用hosts文件模拟dns的功能,从而完成实验

缺省虚拟主机：如果来访者是通过ip来访问,这个时候服务器无法辨别当前要访问的是哪台虚拟主机中的资源,此时访问缺省虚拟主机。缺省虚拟主机可以在server.xml中engin标签上通过defaultHost属性进行配置

## **6.2 访问过程**

![配置虚似主机](http://img.blog.csdn.net/20161030123824919)

# **7. WEB应用程序**

WEB应用程序指供浏览器访问的程序，通常也简称为web应用，是为了提供某一特定功能而按照一定方式组织起来的web资源的组合。

一个web应用由多个静态web资源和动态web资源组成，如

- html、css、js文件
- Jsp文件、java程序、支持jar包、
- 配置文件
- 一个web应用所使用的web资源我们通常使用目录进行组织，这个目录我们通常称为 web应用所在的目录

Web应用开发好后，若想供外界访问，需要把web应用所在目录交给web服务器管理，这个过程称之为虚似目录的映射。

web资源不能直接交给虚拟主机，需要按照功能组织用目录成一个web应用再交给虚拟主机管理

## **7.1 web应用的目录结构**

![web应用的目录结构](http://img.blog.csdn.net/20161030135159777)

静态资源和JSP文件都可以直接放置在web应用的目录下,直接放在web应用下的内容,浏览器可以直接访问到

WEB-INF：可以没有，但是最好有，如果有则一定要保证他的目录结构是完整的。放置在WEB-INF目录下的所有资源浏览器没有办法直接进行访问

classes：动态web资源运行时的class文件要放在这个目录下
lib：动态web资源运行时所依赖的jar包要放在这个目录下
web.xml：整个web应用的配置文件,配置主页/Servlet的映射/过滤器监听器的配置都需要依赖这个文件进行

## **7.2 web.xml文件的作用**

- 某个web资源配置为web应用首页
- 将servlet程序映射到某个url地址上
- 为web应用配置监听器
- 为web应用配置过滤器
- 但凡涉及到对web资源进行配置，都需要通过web.xml文件

## **7.3 web应用的虚拟目录映射**

在server.xml的&lt;Host>标签下配置&lt;Context path="虚拟路径" docBase="真实路径">如果path=""则这个web应用就被配置为了这个虚拟主机的默认web应用

![web应用的虚拟目录映射](http://img.blog.csdn.net/20161030155727758)

在tomcat/conf/引擎名/虚拟主机名 之下建立一个.xml文件，其中文件名用来指定虚拟路径，如果是多级的用#代替/表示，文件中配置&lt;Context docBase="真实目录">，如果文件名起为ROOT.xml则此web应用为默认web应用

直接将web应用放置到虚拟主机对应的目录下，如果目录名起为ROOT则此web应用为默认web应用

如果三处都配置默认，web应用则server.xml > config/.../xx.xml > webapps

## **7.4 其它问题**

- 打war包，方式一：jar -cvf news.war * 方式二：直接用压缩工具压缩为zip包，该后缀为.war
- 通用context和通用web.xml，所有的&lt;Context>都继承子conf/context.xml,所有的web.xml都继承自conf/web.xml
- reloadable让tomcat自动加载更新后的web应用，当java程序修改后不用重启，服务器自动从新加载，开发时设为true方便开发，发布时设为false，提高性能
- Tomcat管理平台，可以在conf/tomcat-users.xml下配置用户名密码及权限

# **8. Tomcat体系架构**

![Tomcat体系架构](http://img.blog.csdn.net/20161030142956575)

# 9. WEB开发的前景

## **9.1 软件开发的两种架构：C/S和B/S**
![C/S和B/S](http://img.blog.csdn.net/20161030130435837)

![C/S和B/S](http://img.blog.csdn.net/20161030130446648)

## **9.2 C/S B/S之争**

![C/S和B/S](http://img.blog.csdn.net/20161030123806067)

![C/S和B/S](http://img.blog.csdn.net/20161030123710019)

讲到这个地方，很多同学就没劲了，为什么呢？搞半天就是做网站哟，没点意思。所以这里特意讲下web开发的前景，免得有些同学像菜鸟一样，以为自己很懂，其实啥都不懂，说些傻话。要讲web开发前景，首先要强调一点，你学javaweb，开发的是程序，别人通过浏览器，访问的是你写的程序，程序为用户完成服务后，再把结果通过写到浏览器中显示，思想不要停留在90年代，以为通过浏览器看到的都是网页。要注意，将来网站都是用来提供服务的，像你们思想中的网页，只有网站提供的一种服务而已。

再者，要讲web开发前景，就不得不提软件开发的两种架构之争了，一种是c/s架构，一种是b/s架构。

何为b/s架构呢？（浏览器/服务器架构）就是指数据和程序都在服务器端，客户端通过浏览器访问程序并获取数据。这种架构的最大好处就是服务器端程序一旦修改，所有客户端访问的都最新的程序，开发人员只管维护服务器就行了，不用管客户端维护的事。这种架构的最大缺点就是，由于客户端都是使用浏览器来访问服务器程序的，数据最终显示在浏览器中，浏览器有多强，数据就能显示成什么样式，数据的显示样式最终由浏览器决定。由于这种特性，所以b/s架构很少用来开发一些对显示有特殊要求的程序，例如游戏，现在的浏览器很难做到把数据显示成一个人，拿着一把刀，到处找人PK，并且还不卡。

何为c/s架构呢？就是指程序运行在客户机上，数据在服务器上。这种架构有一个很大的毛病，就是程序一旦修改，需要更新所有的客户机程序，客户机多，维护的工作量相当恐怖。这种架构的优点是：由于数据的计算在客户机上，服务器的压力小，并且由于数据的显示也由程序员自己编写gui程序完成，显示不受限制。所以c/s架构适合用于开发像游戏这样的程序。

但是，随着网络带宽的不断提升，云计算概念的提出，浏览器只要足够强大，c/s架构立马就会被淘汰，不仅c/s架构会被淘汰，软件最终都会消失、操作系统都可以没有，最终将会是b/s架构的天下，也就是浏览器+搜索引擎的天下。所有现在桌面软件提供的功能，最后都由网站提供，也就是说，将来打开电脑就是一个浏览器，想要什么服务，通过搜索引擎一找，就可以在网上找到相应的服务，用就是了。所以web开发人员是现在最流行的岗位。

![javaweb](http://img.blog.csdn.net/20161103101649551)
