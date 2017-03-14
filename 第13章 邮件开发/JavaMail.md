![javamail](http://img.blog.csdn.net/20161105094658308)

# **1. 邮件协议**

## **1.1 收发邮件**

发邮件大家都会吧！发邮件是从客户端把邮件发送到邮件服务器，收邮件是把邮件服务器的邮件下载到客户端。

![javamail](http://img.blog.csdn.net/20161031190259759)

我们在163、126、QQ、sohu、sina等网站注册的Email账户，其实就是在邮件服务器中注册的。这些网站都有自己的邮件服务器。

## **1.2 邮件协议概述**

与HTTP协议相同，收发邮件也是需要有传输协议的。

- SMTP：（Simple Mail Transfer Protocol，简单邮件传输协议）发邮件协议；
- POP3：（Post Office Protocol Version 3，邮局协议第3版）收邮件协议；
- IMAP：（Internet Message Access Protocol，因特网消息访问协议）收发邮件协议，我们的课程不涉及该协议。

## **1.3 理解邮件收发过程**

其实你可以把邮件服务器理解为邮局！如果你需要给朋友寄一封信，那么你需要把信放到邮筒中，这样你的信会“自动”到达邮局，邮局会把信邮到另一个省市的邮局中。然后这封信会被送到收信人的邮箱中。最终收信人需要自己经常查看邮箱是否有新的信件。

其实每个邮件服务器都由SMTP服务器和POP3服务器构成，其中SMTP服务器负责发邮件的请求，而POP3负责收邮件的请求。

![javamail](http://img.blog.csdn.net/20161031190325431)

当然，有时我们也会使用163的账号，向126的账号发送邮件。这时邮件是发送到126的邮件服务器，而对于163的邮件服务器是不会存储这封邮件的。

![javamail](http://img.blog.csdn.net/20161031190348635)

## **1.4 邮件服务器名称**

smtp服务器的端口号为25，服务器名称为smtp.xxx.xxx。
pop3服务器的端口号为110，服务器名称为pop3.xxx.xxx。

例如：

- 163：smtp.163.com和pop3.163.com；
- 126：smtp.126.com和pop3.126.com；
- qq：smtp.qq.com和pop3.qq.com；
- sohu：smtp.sohu.com和pop3.sohu.com；
- sina：smtp.sina.com和pop3.sina.com。

# **2. Telnet收发邮件**

>Telnet协议是TCP/IP协议族中的一员，是Internet远程登陆服务的标准协议和主要方式。它为用户提供了在本地计算机上完成远程主机工作的能力。在终端使用者的电脑上使用telnet程序，用它连接到服务器。终端使用者可以在telnet程序中输入命令，这些命令会在服务器上运行，就像直接在服务器的控制台上输入一样。可以在本地就能控制服务器。要开始一个telnet会话，必须输入用户名和密码来登录服务器。Telnet是常用的远程控制Web服务器的方法

## **2.1 BASE64加密**

BASE64是一种加密算法，这种加密方式是可逆的！它的作用是使加密后的文本无法用肉眼识别。Java提供了sun.misc.BASE64Encoder这个类，用来对做Base64的加密和解密，但我们知道，使用sun包下的东西会有警告！甚至在eclipse中根本使用不了这个类（需要设置），所以我们还是听sun公司的话，不要去使用它内部使用的类，我们去使用apache commons组件中的codec包下的Base64这个类来完成BASE64加密和解密。

```java
package cn.itcast;
import org.apache.commons.codec.binary.Base64;

public class Base64Utils {
	public static String encode(String s) {
		return encode(s, "utf-8");
	}

	public static String decode(String s) {
		return decode(s, "utf-8");
	}

	public static String encode(String s, String charset) {
		try {
			byte[] bytes = s.getBytes(charset);
			bytes = Base64.encodeBase64(bytes);
			return new String(bytes, charset);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}

	public static String decode(String s, String charset) {
		try {
			byte[] bytes = s.getBytes(charset);
			bytes = Base64.decodeBase64(bytes);
			return new String(bytes, charset);
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```

## **2.2 telnet发邮件**
>Xshell是一个强大的安全终端模拟软件，它支持SSH1, SSH2, 以及Microsoft Windows 平台的TELNET 协议。Xshell 通过互联网到远程主机的安全连接以及它创新性的设计和特色帮助用户在复杂的网络环境中享受他们的工作
>
>Xshell可以在Windows界面下用来访问远端不同系统下的服务器，从而比较好的达到远程控制终端的目的。

连接163的smtp服务器

![javamail](http://img.blog.csdn.net/20161108150708344)

连接成功后需要如下步骤才能发送邮件：

(1)、与服务器打招呼：ehlo你的名字

![javamail](http://img.blog.csdn.net/20161031190444967)

(2)、发出登录请求：auth login

![javamail](http://img.blog.csdn.net/20161031190459170)

(3)、输入加密后的邮箱名：(itcast_cxf@163.com)aXRjYXN0X2N4ZkAxNjMuY29t

(4)、输入加密后的邮箱密码：(itcast)aXRjYXN0

![javamail](http://img.blog.csdn.net/20161031190602418)

(5)、输入谁来发送邮件，即from：mail from:<itcast_cxf@163.com>

![javamail](http://img.blog.csdn.net/20161031190613341)

(6)、输入把邮件发给谁，即to：rcpt to:<itcast_cxf@126.com>

![javamail](http://img.blog.csdn.net/20161031190728311)

(7)、发送填写数据请求：data

![javamail](http://img.blog.csdn.net/20161031190623325)

(8)、开始输入数据，数据包含：from、to、subject，以及邮件内容，如果输入结束后，以一个“.”为一行，表示输入结束：

```
from:<zhangBoZhi@163.com>
to:<itcast_cxf@sina.com>
subject: 我爱上你了

我已经深深的爱上你了，我是张柏芝。
.
```

注意，在标题和邮件正文之间要有一个空行！当要退出时，一定要以一个“.”为单行，表示输入结束。

(9)、最后一步：quit

![javamail](http://img.blog.csdn.net/20161031191029986)

# 3. telnet收邮件

## **3.1 telnet收邮件的步骤**
pop3无需使用Base64加密

收邮件连接的服务器是pop3.xxx.com，pop3协议的默认端口号是110。请注意！这与发邮件完全不同。如果你在163有邮箱账户，那么你想使用telnet收邮件，需要连接的服务器是pop3.163.com

连接pop3服务器：telnet pop3.163.com 110

| 命令     | 功能描述                                     |
| :------ | :---------------------------------------- |
| user命令 | user 用户名，例如：user itcast_cxf@163.com      |
| pass命令 | pass 密码，例如：pass itcast                   |
| stat命令 | stat命令用来查看邮箱中邮件的个数，所有邮件所占的空间             |
| list命令 | list命令用来查看所有邮件，或指定邮件的状态，例如：list 1是查看第一封邮件的大小，<br>list是查看邮件列表，即列出所有邮件的编号，及大小 |
| retr命令 | 查看指定邮件的内容，例如：retr 1#是查看第一封邮件的内容          |
| dele命令 | 标记某邮件为删除，但不是马上删除，而是在退出时才会真正删除            |
| quit命令 | 退出！如果在退出之前已经使用dele命令标记了某些邮件，那么会在退出是删除它们  |
<br>
![javamail](http://img.blog.csdn.net/20161031191101438)

![javamail](http://img.blog.csdn.net/20161031191201423)

![javamail](http://img.blog.csdn.net/20161031191227595)

# **4. JavaMail**

## **4.1 JavaMail概述**

Java Mail是由SUN公司提供的专门针对邮件的API，主要Jar包：mail.jar、activation.jar。

在使用MyEclipse创建web项目时，需要小心！如果只是在web项目中使用java mail是没有什么问题的，发布到Tomcat上运行一点问题都没有！
但是如果是在web项目中写测试那就出问题了。

在MyEclipse中，会自动给web项目导入javax.mail包中的类，但是不全（其实是只有接口，而没有接口的实现类），所以只靠MyEclipse中的类是不能运行java mail项目的，但是如果这时你再去自行导入mail.jar时，就会出现冲突。

处理方案：到下面路径中找到javaee.jar文件，把javax.mail删除！！！
<pre>
D:\Program Files\MyEclipse\Common\plugins\com.genuitec.eclipse.j2eedt.core_10.0.0.me201110301321\data\libraryset\EE_5
</pre>

## **4.2 JavaMail中主要类**

java mail中主要类：javax.mail.Session、javax.mail.internet.MimeMessage、javax.mail.Transport。

Session：表示会话，即客户端与邮件服务器之间的会话！想获得会话需要给出账户和密码，当然还要给出服务器名称。在邮件服务中的Session对象，就相当于连接数据库时的Connection对象。

MimeMessage：表示邮件类，它是Message的子类。它包含邮件的主题（标题）、内容，收件人地址、发件人地址，还可以设置抄送和暗送，甚至还可以设置附件。

Transport：用来发送邮件。它是发送器！

## **4.3 JavaMail之Hello World**

在使用telnet发邮件时，还需要自己来处理Base64编码的问题，但使用JavaMail就不必理会这些问题了，都由JavaMail来处理。

第一步：获得Session

```java
Session session = Session.getInstance(Properties prop, Authenticator auth);

```

其中prop需要指定两个键值，一个是指定服务器主机名，另一个是指定是否需要认证！我们当然需要认证！

```java
Properties prop = new Properties();
prop.setProperty(“mail.host”, “smtp.163.com”);//设置服务器主机名
prop.setProperty(“mail.smtp.auth”, “true”);//设置需要认证
```

其中Authenticator是一个接口表示认证器，即校验客户端的身份。我们需要自己来实现这个接口，实现这个接口需要使用账户和密码。

```java
Authenticator auth = new Authenticator() {
    public PasswordAuthentication getPasswordAuthentication () {
        new PasswordAuthentication(“itcast_cxf”, “itcast”);//用户名和密码
	}
};
```

通过上面的准备，现在可以获取得Session对象了：

```java
Session session = Session.getInstance(prop, auth);
```

第二步：创建MimeMessage对象
创建MimeMessage需要使用Session对象来创建：

```java
MimeMessage msg = new MimeMessage(session);
```

然后需要设置发信人地址、收信人地址、主题，以及邮件正文。

```java
msg.setFrom(new InternetAddress(“itcast_cxf@163.com”));//设置发信人
msg.addRecipients(RecipientType.TO, “itcast_cxf@qq.com,itcast_cxf@sina.com”);//设置多个收信人
msg.addRecipients(RecipientType.CC, “itcast_cxf@sohu.com,itcast_cxf@126.com”);//设置多个抄送
msg.addRecipients(RecipientType.BCC, ”itcast_cxf@hotmail.com”);//设置暗送
msg.setSubject(“这是一封测试邮件”);//设置主题（标题）
msg.setContent(“当然是hello world!”, “text/plain;charset=utf-8”);//设置正文
```
第三步：发送邮件
```java
Transport.send(msg);//发送邮件
```
## **4.4 JavaMail发送带有附件的邮件**

一封邮件可以包含正文、附件N个，所以正文与N个附件都是邮件的一个部份。

上面的hello world案例中，只是发送了带有正文的邮件！所以在调用setContent()方法时直接设置了正文，如果想发送带有附件邮件，那么需要设置邮件的内容为MimeMultiPart。

```java
MimeMulitpart parts = new MimeMulitpart();//多部件对象，可以理解为是部件的集合
msg.setContent(parts);//设置邮件的内容为多部件内容。
```

然后我们需要把正文、N个附件创建为“主体部件”对象（MimeBodyPart），添加到MimeMuiltPart中即可。

```java
MimeBodyPart part1 = new MimeBodyPart();//创建一个部件
part1.setCotnent(“这是正文部分”, “text/html;charset=utf-8”);//给部件设置内容
parts.addBodyPart(part1);//把部件添加到部件集中。

```

下面我们创建一个附件：

```java
MimeBodyPart part2 = new MimeBodyPart();//创建一个部件
part2.attachFile(“F:\\a.jpg”);//设置附件
part2.setFileName(“hello.jpg”);//设置附件名称
parts.addBodyPart(part2);//把附件添加到部件集中
```

注意，如果在设置文件名称时，文件名称中包含了中文的话，那么需要使用MimeUitlity类来给中文编码：

```java
part2.setFileName(MimeUitlity.encodeText(“美女.jpg”));
```

![JavaMail](http://img.blog.csdn.net/20161031191305536)

```java
public class JavaMailDemo {

  public void sendMail() throws Exception {
    /*
     * 1. 得到session
     */
    Properties props = new Properties();
    props.setProperty("mail.host", "smtp.163.com");
    props.setProperty("mail.smtp.auth", "true");

    Authenticator auth = new Authenticator() {
      @Override
      protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication("itcast_cxf", "itcast");
      }
    };

    Session session = Session.getInstance(props, auth);

    /*
     * 2. 创建MimeMessage
     */
    MimeMessage msg = new MimeMessage(session);
    msg.setFrom(new InternetAddress("itcast_cxf@163.com"));//设置发件人
    msg.setRecipients(RecipientType.TO, "itcast_cxf@126.com");//设置收件人
    msg.setRecipients(RecipientType.CC, "itcast_cxf@sohu.com");//设置抄送
    msg.setRecipients(RecipientType.BCC, "itcast_cxf@sina.com");//设置暗送

    msg.setSubject("这是来自ITCAST的测试邮件");
    msg.setContent("这就是一封垃圾邮件！", "text/html;charset=utf-8");

    /*
     * 3. 发邮件
     */
    Transport.send(msg);
  }

  /**
   * 带有附件的邮件！！！
   */

  public void sendMail2() throws Exception {
    /*
     * 1. 得到session
     */
    Properties props = new Properties();
    props.setProperty("mail.host", "smtp.163.com");
    props.setProperty("mail.smtp.auth", "true");

    Authenticator auth = new Authenticator() {
      @Override
      protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication("itcast_cxf", "itcast");
      }
    };

    Session session = Session.getInstance(props, auth);

    /*
     * 2. 创建MimeMessage
     */
    MimeMessage msg = new MimeMessage(session);
    msg.setFrom(new InternetAddress("itcast_cxf@163.com"));//设置发件人
    msg.setRecipients(RecipientType.TO, "itcast_cxf@126.com");//设置收件人

    msg.setSubject("这是来自ITCAST的测试邮件有附件");


    ////////////////////////////////////////////////////////
    /*
     * 当发送包含附件的邮件时，邮件体就为多部件形式！
     * 1. 创建一个多部件的部件内容！MimeMultipart
     *   MimeMultipart就是一个集合，用来装载多个主体部件！
     * 2. 我们需要创建两个主体部件，一个是文本内容的，另一个是附件的。
     *   主体部件叫MimeBodyPart
     * 3. 把MimeMultipart设置给MimeMessage的内容！
     */
    MimeMultipart list = new MimeMultipart();//创建多部分内容

    // 创建MimeBodyPart
    MimeBodyPart part1 = new MimeBodyPart();
    // 设置主体部件的内容
    part1.setContent("这是一封包含附件的垃圾邮件", "text/html;charset=utf-8");
    // 把主体部件添加到集合中
    list.addBodyPart(part1);


    // 创建MimeBodyPart
    MimeBodyPart part2 = new MimeBodyPart();
    part2.attachFile(new File("F:/f/白冰.jpg"));//设置附件的内容
    //设置显示的文件名称，其中encodeText用来处理中文乱码问题
    part2.setFileName(MimeUtility.encodeText("大美女.jpg"));
    list.addBodyPart(part2);

    msg.setContent(list);//把它设置给邮件作为邮件的内容。


    ////////////////////////////////////////////////////////

    /*
     * 3. 发邮件
     */
    Transport.send(msg);    
  }

  public void sendMail3() throws Exception {
    /*
     * 1. 得到session
     */
    Session session = MailUtils.createSession("smtp.163.com",
        "itcast_cxf", "itcast");
    /*
     * 2. 创建邮件对象
     */
    Mail mail = new Mail("itcast_cxf@163.com",
        "itcast_cxf@126.com,itcast_cxf@sina.com",
        "不是垃圾邮件能是什么呢？", "这里是正文");

    /*
     * 创建两个附件对象
     */
    AttachBean ab1 = new AttachBean(new File("F:/f/白冰.jpg"), "小美女.jpg");
    AttachBean ab2 = new AttachBean(new File("F:/f/big.jpg"), "我的羽绒服.jpg");

    // 添加到mail中
    mail.addAttach(ab1);
    mail.addAttach(ab2);

    /*
     * 3. 发送
     */
    MailUtils.send(session, mail);
  }
}
```
# **5. MailUtils**

```java
public class MailUtils {
	public static Session createSession(String host, final String username, final String password) {
		Properties prop = new Properties();
		prop.setProperty("mail.host", host);// 指定主机
		prop.setProperty("mail.smtp.auth", "true");// 指定验证为true

		// 创建验证器
		Authenticator auth = new Authenticator() {
			public PasswordAuthentication getPasswordAuthentication() {
				return new PasswordAuthentication(username, password);
			}
		};

		// 获取session对象
		return Session.getInstance(prop, auth);
	}

	/**
	 * 发送指定的邮件
	 *
	 * @param mail
	 */
	public static void send(Session session, final Mail mail) throws MessagingException,
			IOException {

		MimeMessage msg = new MimeMessage(session);// 创建邮件对象
		msg.setFrom(new InternetAddress(mail.getFrom()));// 设置发件人
		msg.addRecipients(RecipientType.TO, mail.getToAddress());// 设置收件人

		// 设置抄送
		String cc = mail.getCcAddress();
		if (!cc.isEmpty()) {
			msg.addRecipients(RecipientType.CC, cc);
		}

		// 设置暗送
		String bcc = mail.getBccAddress();
		if (!bcc.isEmpty()) {
			msg.addRecipients(RecipientType.BCC, bcc);
		}

		msg.setSubject(mail.getSubject());// 设置主题

		MimeMultipart parts = new MimeMultipart();// 创建部件集对象

		MimeBodyPart part = new MimeBodyPart();// 创建一个部件
		part.setContent(mail.getContent(), "text/html;charset=utf-8");// 设置邮件文本内容
		parts.addBodyPart(part);// 把部件添加到部件集中

		///////////////////////////////////////////

		// 添加附件
		List<AttachBean> attachBeanList = mail.getAttachs();// 获取所有附件
		if (attachBeanList != null) {
			for (AttachBean attach : attachBeanList) {
				MimeBodyPart attachPart = new MimeBodyPart();// 创建一个部件
				attachPart.attachFile(attach.getFile());// 设置附件文件
				attachPart.setFileName(MimeUtility.encodeText(attach
						.getFileName()));// 设置附件文件名
				parts.addBodyPart(attachPart);
			}
		}

		msg.setContent(parts);// 给邮件设置内容
		Transport.send(msg);// 发邮件
	}
}
```
