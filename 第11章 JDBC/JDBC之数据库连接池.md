![jdbc](http://img.blog.csdn.net/20161104231600003)
# **JDBC系列阅读**
1. [JavaWeb：用JDBC操作数据库](http://blog.csdn.net/axi295309066/article/details/52954659)
2. [JavaWeb：JDBC之事务](http://blog.csdn.net/axi295309066/article/details/52981430)
3. [JavaWeb：JDBC之数据库连接池](http://blog.csdn.net/axi295309066/article/details/52981389)

# 1. 池参数（所有池参数都有默认值）
- 初始大小：10个
- 最小空闲连接数：3个
- 增量：一次创建的最小单位（5个）
- 最大空闲连接数：12个
- 最大连接数：20个
- 最大的等待时间：1000毫秒

# 2. 四大连接参数
连接池也是使用四大连接参数来完成创建连接对象！

# 3. 实现的接口
连接池必须实现：javax.sql.DataSource接口！

连接池返回的Connection对象，它的close()方法与众不同！调用它的close()不是关闭，而是把连接归还给池！

# 4. 数据库连接池

## **4.1 数据库连接池的概念**
用池来管理Connection，这可以重复使用Connection。有了池，所以我们就不用自己来创建Connection，而是通过池来获取Connection对象。当使用完Connection后，调用Connection的close()方法也不会真的关闭Connection，而是把Connection“归还”给池。池就可以再利用这个Connection对象了

![jdbc](http://img.blog.csdn.net/20161031121829027)
## **4.2、JDBC数据库连接池接口（DataSource）**
Java为数据库连接池提供了公共的接口：javax.sql.DataSource，各个厂商可以让自己的连接池实现这个接口。这样应用程序可以方便的切换不同厂商的连接池！

## **4.3、自定义连接池（ItcastPool）**
分析：ItcastPool需要有一个List，用来保存连接对象。在ItcastPool的构造器中创建5个连接对象放到List中！当用人调用了ItcastPool的getConnection()时，那么就从List拿出一个返回。当List中没有连接可用时，抛出异常

我们需要对Connection的close()方法进行增强，所以我们需要自定义ItcastConnection类，对Connection进行装饰！即对close()方法进行增强。因为需要在调用close()方法时把连接“归还”给池，所以ItcastConnection类需要拥有池对象的引用，并且池类还要提供“归还”的方法

![jdbc](http://img.blog.csdn.net/20161031123148801)

ItcastPool.java

```java
public class ItcastPool implements DataSource {
	private static Properties props = new Properties();
	private List<Connection> list = new ArrayList<Connection>();
	static {
		InputStream in = ItcastPool.class.getClassLoader()
				.getResourceAsStream("dbconfig.properties");
		try {
			props.load(in);
			Class.forName(props.getProperty("driverClassName"));
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}

	public ItcastPool() throws SQLException {
		for (int i = 0; i < 5; i++) {
			Connection con = DriverManager.getConnection(
					props.getProperty("url"), props.getProperty("username"),
					props.getProperty("password"));
			ItcastConnection conWapper = new ItcastConnection(con, this);
			list.add(conWapper);
		}
	}

	public void add(Connection con) {
		list.add(con);
	}

	public Connection getConnection() throws SQLException {
		if(list.size() > 0) {
			return list.remove(0);
		}
		throw new SQLException("没连接了");
	}
    ......
}
```

ItcastConnection.java

```java
public class ItcastConnection extends ConnectionWrapper {
	private ItcastPool pool;

	public ItcastConnection(Connection con, ItcastPool pool) {
		super(con);
		this.pool = pool;
	}

	@Override
	public void close() throws SQLException {
		pool.add(this);
	}
}
```

# 5. DBCP

## **5.1、什么是DBCP？**
DBCP是Apache提供的一款开源免费的数据库连接池！

Hibernate3.0之后不再对DBCP提供支持！因为Hibernate声明DBCP有致命的缺欠！DBCP因为Hibernate的这一毁谤很是生气，并且说自己没有缺欠

## **5.2、DBCP的使用**

```java
public void fun1() throws SQLException {
  BasicDataSource ds = new BasicDataSource();
  ds.setUsername("root");
  ds.setPassword("123");
  ds.setUrl("jdbc:mysql://localhost:3306/mydb1");
  ds.setDriverClassName("com.mysql.jdbc.Driver");

  ds.setMaxActive(20);
  ds.setMaxIdle(10);
  ds.setInitialSize(10);
  ds.setMinIdle(2);
  ds.setMaxWait(1000);

  Connection con = ds.getConnection();
  System.out.println(con.getClass().getName());
  con.close();
}
```

## **5.3、DBCP的配置信息**
下面是对DBCP的配置介绍：

```
#基本配置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mydb1
username=root
password=123

#初始化池大小，即一开始池中就会有10个连接对象
默认值为0
initialSize=0

#最大连接数，如果设置maxActive=50时，池中最多可以有50个连接，当然这50个连接中包含被使用的和没被使用的（空闲）
#你是一个包工头，你一共有50个工人，但这50个工人有的当前正在工作，有的正在空闲
#默认值为8，如果设置为非正数，表示没有限制！即无限大
maxActive=8

#最大空闲连接
#当设置maxIdle=30时，你是包工头，你允许最多有20个工人空闲，如果现在有30个空闲工人，那么要开除10个
#默认值为8，如果设置为负数，表示没有限制！即无限大
maxIdle=8

#最小空闲连接
#如果设置minIdel=5时，如果你的工人只有3个空闲，那么你需要再去招2个回来，保证有5个空闲工人
#默认值为0
minIdle=0

#最大等待时间
#当设置maxWait=5000时，现在你的工作都出去工作了，又来了一个工作，需要一个工人。
#这时就要等待有工人回来，如果等待5000毫秒还没回来，那就抛出异常
#没有工人的原因：最多工人数为50，已经有50个工人了，不能再招了，但50人都出去工作了。
#默认值为-1，表示无限期等待，不会抛出异常。
maxWait=-1

#连接属性
#就是原来放在url后面的参数，可以使用connectionProperties来指定
#如果已经在url后面指定了，那么就不用在这里指定了。
#useServerPrepStmts=true，MySQL开启预编译功能
#cachePrepStmts=true，MySQL开启缓存PreparedStatement功能，
#prepStmtCacheSize=50，缓存PreparedStatement的上限
#prepStmtCacheSqlLimit=300，当SQL模板长度大于300时，就不再缓存它
connectionProperties=useUnicode=true;characterEncoding=UTF8;useServerPrepStmts=true;cachePrepStmts=true;prepStmtCacheSize=50;prepStmtCacheSqlLimit=300

#连接的默认提交方式
#默认值为true
defaultAutoCommit=true

#连接是否为只读连接
#Connection有一对方法：setReadOnly(boolean)和isReadOnly()
#如果是只读连接，那么你只能用这个连接来做查询
#指定连接为只读是为了优化！这个优化与并发事务相关！
#如果两个并发事务，对同一行记录做增、删、改操作，是不是一定要隔离它们啊？
#如果两个并发事务，对同一行记录只做查询操作，那么是不是就不用隔离它们了？
#如果没有指定这个属性值，那么是否为只读连接，这就由驱动自己来决定了。即Connection的实现类自己来决定！
defaultReadOnly=false

#指定事务的事务隔离级别
#可选值：NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
#如果没有指定，那么由驱动中的Connection实现类自己来决定
defaultTransactionIsolation=REPEATABLE_READ
```

# 6. C3P0

## **6.1、C3P0简介**
C3P0也是开源免费的连接池！C3P0被很多人看好！

## **6.2、C3P0的使用**
C3P0中池类是：ComboPooledDataSource。

```java
public void fun1() throws PropertyVetoException, SQLException {
  ComboPooledDataSource ds = new ComboPooledDataSource();
  ds.setJdbcUrl("jdbc:mysql://localhost:3306/mydb1");
  ds.setUser("root");
  ds.setPassword("123");
  ds.setDriverClass("com.mysql.jdbc.Driver");

  ds.setAcquireIncrement(5);
  ds.setInitialPoolSize(20);
  ds.setMinPoolSize(2);
  ds.setMaxPoolSize(50);

  Connection con = ds.getConnection();
  System.out.println(con);
  con.close();
}
```

配置文件要求：

- 文件名称：必须叫c3p0-config.xml
- 文件位置：必须在src下


c3p0也可以指定配置文件，而且配置文件可以是properties，也可骒xml的。当然xml的高级一些了。但是c3p0的配置文件名必须为c3p0-config.xml，并且必须放在类路径下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<default-config>
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/mydb1</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">123</property>
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</default-config>
	<named-config name="oracle-config">
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/mydb1</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">123</property>
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</named-config>
</c3p0-config>
```

c3p0的配置文件中可以配置多个连接信息，可以给每个配置起个名字，这样可以方便的通过配置名称来切换配置信息。上面文件中默认配置为mysql的配置，名为oracle-config的配置也是mysql的配置，呵呵

```java
public void fun2() throws PropertyVetoException, SQLException {
  ComboPooledDataSource ds = new ComboPooledDataSource();
  Connection con = ds.getConnection();
  System.out.println(con);
  con.close();
}

public void fun2() throws PropertyVetoException, SQLException {
  ComboPooledDataSource ds = new ComboPooledDataSource("orcale-config");
  Connection con = ds.getConnection();
  System.out.println(con);
  con.close();
}
```

# 7. Tomcat配置连接池

## **7.1、Tomcat配置JNDI资源**
JNDI（Java Naming and Directory Interface），Java命名和目录接口。JNDI的作用就是：在服务器上配置资源，然后通过统一的方式来获取配置的资源

我们这里要配置的资源当然是连接池了，这样项目中就可以通过统一的方式来获取连接池对象了

下图是Tomcat文档提供的：

![jdbc](http://img.blog.csdn.net/20161031123324485)

配置JNDI资源需要到&lt;Context>元素中配置&lt;Resource>子元素：

- name：指定资源的名称，这个名称可以随便给，在获取资源时需要这个名称
- factory：用来创建资源的工厂，这个值基本上是固定的，不用修改
- type：资源的类型，我们要给出的类型当然是我们连接池的类型了
- bar：表示资源的属性，如果资源存在名为bar的属性，那么就配置bar的值。对于DBCP连接池而言，你需要配置的不是bar，因为它没有bar这个属性，而是应该去配置url、username等属性

```xml
<Context>  
  <Resource name="mydbcp"
			type="org.apache.tomcat.dbcp.dbcp.BasicDataSource"
			factory="org.apache.naming.factory.BeanFactory"
			username="root"
			password="123"
			driverClassName="com.mysql.jdbc.Driver"    
			url="jdbc:mysql://127.0.0.1/mydb1"
			maxIdle="3"
			maxWait="5000"
			maxActive="5"
			initialSize="3"/>
</Context>  
<Context>  
  <Resource name="myc3p0"
			type="com.mchange.v2.c3p0.ComboPooledDataSource"
			factory="org.apache.naming.factory.BeanFactory"
			user="root"
			password="123"
			classDriver="com.mysql.jdbc.Driver"    
			jdbcUrl="jdbc:mysql://127.0.0.1/mydb1"
			maxPoolSize="20"
			minPoolSize ="5"
			initialPoolSize="10"
			acquireIncrement="2"/>
</Context>  
```

## **7.2、获取资源**
配置资源的目的当然是为了获取资源了。只要你启动了Tomcat，那么就可以在项目中任何类中通过JNDI获取资源的方式来获取资源了

下图是Tomcat文档提供的，与上面Tomcat文档提供的配置资源是对应的。

![jdbc](http://img.blog.csdn.net/20161031123222238)

获取资源：

- Context：javax.naming.Context
- InitialContext：javax.naming.InitialContext
- lookup(String)：获取资源的方法，其中”java:comp/env”是资源的入口（这是固定的名称），获取过来的还是一个Context，这说明需要在获取到的Context上进一步进行获取。”bean/MyBeanFactory”对应&lt;Resource>中配置的name值，这回获取的就是资源对象了

```java
Context cxt = new InitialContext();
DataSource ds = (DataSource)cxt.lookup("java:/comp/env/mydbcp");
Connection con = ds.getConnection();
System.out.println(con);
con.close();
Context cxt = new InitialContext();
Context envCxt = (Context)cxt.lookup("java:/comp/env");
DataSource ds = (DataSource)env.lookup("mydbcp");
Connection con = ds.getConnection();
System.out.println(con);
con.close();
```

上面两种方式是相同的效果

## **7.3 修改JdbcUtils**

因为已经学习了连接池，那么JdbcUtils的获取连接对象的方法也要修改一下了。

JdbcUtils.java

```java
public class JdbcUtils {
	private static DataSource dataSource = new ComboPooledDataSource();

	public static DataSource getDataSource() {
		return dataSource;
	}

	public static Connection getConnection() {
		try {
			return dataSource.getConnection();
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```

# 8. ThreadLocal

Thread ->人类
Runnable -> 任务类

| key     | value |
| :------ | :---- |
| thread1 | aaa   |
| thread2 | bbb   |
| thread3 | ccc   |

## **8.1、ThreadLocal API**
ThreadLocal类只有三个方法

| 返回值  | 方法说明         | 功能描述 |
| :--- | :----------- | :--- |
| void | set(T value) | 保存值  |
| T    | get()        | 获取值  |
| void | remove()     | 移除值  |

## **8.2、ThreadLocal的内部是Map**
ThreadLocal内部其实是个Map来保存数据。虽然在使用ThreadLocal时只给出了值，没有给出键，其实它内部使用了当前线程做为键

```java
class MyThreadLocal<T> {
	private Map<Thread,T> map = new HashMap<Thread,T>();
	public void set(T value) {
		map.put(Thread.currentThread(), value);
	}

	public void remove() {
		map.remove(Thread.currentThread());
	}

	public T get() {
		return map.get(Thread.currentThread());
	}
}
```

 ![jdbc](http://img.blog.csdn.net/20161031123355223)

# 9. BaseServlet

## **9.1、BaseServlet的作用**

在开始客户管理系统之前，我们先写一个工具类：BaseServlet

我们知道，写一个项目可能会出现N多个Servlet，而且一般一个Servlet只有一个方法（doGet或doPost），如果项目大一些，那么Servlet的数量就会很惊人

为了避免Servlet的“膨胀”，我们写一个BaseServlet。它的作用是让一个Servlet可以处理多种不同的请求。不同的请求调用Servlet的不同方法。我们写好了BaseServlet后，让其他Servlet继承BaseServlet，例如CustomerServlet继承BaseServlet，然后在CustomerServlet中提供add()、update()、delete()等方法，每个方法对应不同的请求。

![jdbc](http://img.blog.csdn.net/20161031123412221)
## **9.2、BaseServlet分析**
我们知道，Servlet中处理请求的方法是service()方法，这说明我们需要让service()方法去调用其他方法。例如调用add()、mod()、del()、all()等方法！具体调用哪个方法需要在请求中给出方法名称！然后service()方法通过方法名称来调用指定的方法

无论是点击超链接，还是提交表单，请求中必须要有method参数，这个参数的值就是要请求的方法名称，这样BaseServlet的service()才能通过方法名称来调用目标方法。例如某个链接如下：

```html
<a href=”/xxx/CustomerServlet?method=add”>添加客户</a>
```


![jdbc](http://img.blog.csdn.net/20161031123423177)

## **9.3、BaseServlet代码**

```java
public class BaseServlet extends HttpServlet {
	/*
	 * 它会根据请求中的m，来决定调用本类的哪个方法
	 */
	protected void service(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException {
		req.setCharacterEncoding("UTF-8");
		res.setContentType("text/html;charset=utf-8");

		// 例如：http://localhost:8080/demo1/xxx?m=add
		String methodName = req.getParameter("method");// 它是一个方法名称

		// 当没用指定要调用的方法时，那么默认请求的是execute()方法。
		if(methodName == null || methodName.isEmpty()) {
			methodName = "execute";
		}
		Class c = this.getClass();
		try {
			// 通过方法名称获取方法的反射对象
			Method m = c.getMethod(methodName, HttpServletRequest.class,
					HttpServletResponse.class);
			// 反射方法目标方法，也就是说，如果methodName为add，那么就调用add方法。
			String result = (String) m.invoke(this, req, res);
			// 通过返回值完成请求转发
			if(result != null && !result.isEmpty()) {
				req.getRequestDispatcher(result).forward(req, res);
			}
		} catch (Exception e) {
			throw new ServletException(e);
		}
	}
}
```

# 10. DBUtils

## **10.1、DBUtils简介**
DBUtils是Apache Commons组件中的一员，开源免费！
DBUtils是对JDBC的简单封装，但是它还是被很多公司使用！
DBUtils的Jar包：dbutils.jar

## **10.2、DBUtils主要类**
- DbUtils：都是静态方法，一系列的close()方法；
- QueryRunner：
- update()：执行insert、update、delete
- query()：执行select语句
- batch()：执行批处理

## **10.3、QueryRunner**
### **update()方法**
QueryRunner的update()方法可以用来执行insert、update、delete语句。

- 无参构造QueryRunner()

```java
//可执行增、删、改语句，需要调用者提供Connection，这说明本方法不再管理Connection了。支持事务!
int update(Connection con, String sql, Object… params);
```

```java
	@Test
	public void fun1() throws SQLException {
		QueryRunner qr = new QueryRunner();
		String sql = "insert into user values(?,?,?)";
		qr.update(JdbcUtils.getConnection(), sql, "u1", "zhangSan", "123");
	}
```
还有另一种方式来使用QueryRunner，QueryRunner(DataSource)带连接池的构造，这种方式在创建QueryRunner时传递了连接池对象，那么在调用update()方法时就不用再传递Connection了

```java
//可执行增、删、改语句
int update(String sql, Object… params);
```

```java
	@Test
	public void fun2() throws SQLException {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "insert into user values(?,?,?)";
		qr.update(sql, "u1", "zhangSan", "123");
	}
```

###**query()方法**

```java
//可执行查询，它会先得到ResultSet，然后调用rsh的handle()把rs转换成需要的类型！
T query(String sql, ResultSetHandler rsh, Object... params);

//支持事务
T query(Connection con, String sql, ResultSetHadler rsh, Object... params);
```
## **10.4、ResultSetHandler**
我们知道在执行select语句之后得到的是ResultSet，然后我们还需要对ResultSet进行转换，得到最终我们想要的数据。你可以希望把ResultSet的数据放到一个List中，也可能想把数据放到一个Map中，或是一个Bean中

DBUtils提供了一个接口ResultSetHandler，它就是用来ResultSet转换成目标类型的工具。你可以自己去实现这个接口，把ResultSet转换成你想要的类型

DBUtils提供了很多个ResultSetHandler接口的实现，这些实现已经基本够用了，我们通常不用自己去实现ResultSet接口了

| 处理器               | 功能描述                                     |
| :---------------- | :--------------------------------------- |
| MapHandler        | 单行处理器！把结果集转换成Map&lt;String,Object>，其中列名为键 |
| MapListHandler    | 多行处理器！把结果集转换成List&lt;Map&lt;String,Object>> |
| BeanHandler       | 单行处理器！把结果集转换成Bean，该处理器需要Class参数，即Bean的类型 |
| BeanListHandler   | 多行处理器！把结果集转换成List&lt;Bean>               |
| ColumnListHandler | 多行单列处理器！把结果集转换成List&lt;Object>，使用ColumnListHandler时需要指定某一列的名称或编号，例如：new ColumListHandler(“name”)表示把name列的数据放到List中 |
| ScalarHandler     | 单行单列处理器！把结果集转换成Object。一般用于聚集查询，例如select count(*) from tab_student |
<br>
Map处理器

![jdbc](http://img.blog.csdn.net/20161031123454427)

Bean处理器

![jdbc](http://img.blog.csdn.net/20161031123506271)

Column处理器

 ![jdbc](http://img.blog.csdn.net/20161031123515239)

Scalar处理器

 ![jdbc](http://img.blog.csdn.net/20161031123523505)

## **10.5、QueryRunner之查询**
QueryRunner的查询方法是：

```java
public <T> T query(String sql, ResultSetHandler<T> rh, Object… params)
public <T> T query(Connection con, String sql, ResultSetHandler<T> rh, Object… params)
```

query()方法会通过sql语句和params查询出ResultSet，然后通过rh把ResultSet转换成对应的类型再返回

```java
@Test
public void fun1() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select * from tab_student where number=?";
  Map<String,Object> map = qr.query(sql, new MapHandler(), "S_2000");
  System.out.println(map);
}

@Test
public void fun2() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select * from tab_student";
  List<Map<String,Object>> list = qr.query(sql, new MapListHandler());
  for(Map<String,Object> map : list) {
    System.out.println(map);
  }
}

@Test
public void fun3() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select * from tab_student where number=?";
  Student stu = qr.query(sql, new BeanHandler<Student>(Student.class), "S_2000");
  System.out.println(stu);
}

@Test
public void fun4() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select * from tab_student";
  List<Student> list = qr.query(sql, new BeanListHandler<Student>(Student.class));
  for(Student stu : list) {
    System.out.println(stu);
  }
}

@Test
public void fun5() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select * from tab_student";
  List<Object> list = qr.query(sql, new ColumnListHandler("name"));
  for(Object s : list) {
    System.out.println(s);
  }
}

@Test
public void fun6() throws SQLException {
  DataSource ds = JdbcUtils.getDataSource();
  QueryRunner qr = new QueryRunner(ds);
  String sql = "select count(*) from tab_student";
  Number number = (Number)qr.query(sql, new ScalarHandler());
  int cnt = number.intValue();
  System.out.println(cnt);
}
```

## **10.6、QueryRunner之批处理**
QueryRunner还提供了批处理方法：batch()

我们更新一行记录时需要指定一个Object[]为参数，如果是批处理，那么就要指定Object[][]为参数了。即多个Object[]就是Object[][]了，其中每个Object[]对应一行记录：

```java
@Test
public void fun10() throws SQLException {
	DataSource ds = JdbcUtils.getDataSource();
	QueryRunner qr = new QueryRunner(ds);
	String sql = "insert into tab_student values(?,?,?,?)";
	Object[][] params = new Object[10][];//表示 要插入10行记录
	for(int i = 0; i < params.length; i++) {
		params[i] = new Object[]{"S_300" + i, "name" + i, 30 + i, i%2==0?"男":"女"};
	}
	qr.batch(sql, params);
}
```
# 11. Service事务
在Service中使用ThreadLocal来完成事务，为将来学习Spring事务打基础！

## **11.1、DAO中的事务**
在DAO中处理事务真是“小菜一碟”。

```java
public void xxx() {
   Connection con = null;
   try {
      con = JdbcUtils.getConnection();
      con.setAutoCommitted(false);
      QueryRunner qr = new QueryRunner();
      String sql = …;
      Object[] params = …;
      qr.update(con, sql, params);

	  sql = …;
      Object[] params = …;
      qr.update(con, sql, params);
      con.commit();
} catch(Exception e) {
    try {
       if(con != null) {con.rollback();}
} catch(Exception e) {}
} finally {
    try {
       con.close();
} catch(Exception e) {}
}
}
```
## **11.2、Service才是处理事务的地方**
我们要清楚一件事，DAO中不是处理事务的地方，因为DAO中的每个方法都是对数据库的一次操作，而Service中的方法才是对应一个业务逻辑。也就是说我们需要在Service中的一方法中调用DAO的多个方法，而这些方法应该在一起事务中。

怎么才能让DAO的多个方法使用相同的Connection呢？方法不能再自己来获得Connection，而是由外界传递进去。

```java
public void daoMethod1(Connection con, …) {
}
public void daoMethod2(Connection con, …) {
}
```

在Service中调用DAO的多个方法时，传递相同的Connection就可以了。

```java
public class XXXService() {
   private XXXDao dao = new XXXDao();
   public void serviceMethod() {
      Connection con = null;
      try {
         con = JdbcUtils.getConnection();
         con.setAutoCommitted(false);
         dao.daoMethod1(con, …);
         dao.doaMethod2(con, …);
         com.commint();
	} catch(Exception e) {
   try {
      con.rollback();
	} catch(Exception e) {}
	} finally {
   try {
     con.close();
   } catch(Exception e) {}
	}
  }
}
```

但是，在Service中不应该出现Connection，它应该只在DAO中出现，因为它是JDBC的东西，JDBC的东西是用来连接数据库的，连接数据库是DAO的事儿！！！但是，事务是Service的事儿，不能放到DAO中！！！

## **11.3、修改JdbcUtils**
我们把对事务的开启和关闭放到JdbcUtils中，在Service中调用JdbcUtils的方法来完成事务的处理，但在Service中就不会再出现Connection这一“禁忌”了。

DAO中的方法不用再让Service来传递Connection了。DAO会主动从JdbcUtils中获取Connection对象，这样，JdbcUtils成为了DAO和Service的中介！

我们在JdbcUtils中添加beginTransaction()和rollbackTransaction()，以及commitTransaction()方法。这样在Service中的代码如下：

```java
public class XXXService() {
   private XXXDao dao = new XXXDao();
   public void serviceMethod() {
   try {
      JdbcUtils.beginTransaction();
      dao.daoMethod1(…);
      dao.daoMethod2(…);
      JdbcUtils.commitTransaction();
		} catch(Exception e) {
   JdbcUtils.rollbackTransaction();
	}
  }
}
```

```java
DAO
public void daoMethod1(…) {
  Connection con = JdbcUtils.getConnection();
}
public void daoMethod2(…) {
  Connection con = JdbcUtils.getConnection();
}
```

在Service中调用了JdbcUtils.beginTransaction()方法时，JdbcUtils要做准备好一个已经调用了setAuthCommitted(false)方法的Connection对象，因为在Service中调用JdbcUtils.beginTransaction()之后，马上就会调用DAO的方法，而在DAO方法中会调用JdbcUtils.getConnection()方法。这说明JdbcUtils要在getConnection()方法中返回刚刚准备好的，已经设置了手动提交的Connection对象。

 ![jdbc](http://img.blog.csdn.net/20161102182901093)

在JdbcUtils中创建一个Connection con属性，当它为null时，说明没有事务！当它不为null时，表示开启了事务。

- 在没有开启事务时，可以调用“开启事务”方法；
- 在开启事务后，可以调用“提交事务”和“回滚事务”方法；
- getConnection()方法会在con不为null时返回con，再con为null时，从连接池中返回连接。

**beginTransaction()**

判断con是否为null，如果不为null，就抛出异常！
如果con为null，那么从连接池中获取一个Connection对象，赋值给con！然后设置它为“手动提交”。

**getConnection()**

判断con是否为null，如果为null说明没有事务，那么从连接池获取一个连接返回；
如果不为null，说明已经开始了事务，那么返回con属性返回。这说明在con不为null时，无论调用多少次getConnection()方法，返回的都是同个Connection对象。

**commitTransaction()**

判断con是否为null，如果为null，说明没有开启事务就提交事务，那么抛出异常；
如果con不为null，那么调用con的commit()方法来提交事务；
调用con.close()方法关闭连接；
con = null，这表示事务已经结束！

**rollbackTransaction()**

判断con是否为null，如果为null，说明没有开启事务就回滚事务，那么抛出异常；
如果con不为null，那么调用con的rollback()方法来回滚事务；
调用con.close()方法关闭连接；
con = null，这表示事务已经结束！

JdbcUtils.java

```java
public class JdbcUtils {
	private static DataSource dataSource = new ComboPooledDataSource();
	private static Connection con = null;

	public static DataSource getDataSource() {
		return dataSource;
	}

	public static Connection getConnection() throws SQLException {
		if(con == null) {
			return dataSource.getConnection();
		}
		return con;
	}

	public static void beginTranscation() throws SQLException {
		if(con != null) {
			throw new SQLException("事务已经开启，在没有结束当前事务时，不能再开启事务！");
		}
		con = dataSource.getConnection();
		con.setAutoCommit(false);
	}

	public static void commitTransaction() throws SQLException {
		if(con == null) {
			throw new SQLException("当前没有事务，所以不能提交事务！");
		}
		con.commit();
		con.close();
		con = null;
	}

	public static void rollbackTransaction() throws SQLException {
		if(con == null) {
			throw new SQLException("当前没有事务，所以不能回滚事务！");
		}
		con.rollback();
		con.close();
		con = null;		
	}
}
```

## **11.4、再次修改JdbcUtils**
现在JdbcUtils有个问题，如果有两个线程！第一个线程调用了beginTransaction()方法，另一个线程再调用beginTransaction()方法时，因为con已经不再为null，所以就会抛出异常了。

我们希望JdbcUtils可以多线程环境下被使用！这说明最好的方法是为每个线程提供一个Connection，这样每个线程都可以开启自己的事务了。
还记得ThreadLocal类么？

```java
public class JdbcUtils {
	// 配置文件的默认配置！要求你必须给出c3p0-config.xml！！！
	private static ComboPooledDataSource dataSource = new ComboPooledDataSource();

	// 它是事务专用连接！
	private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

	/**
	 * 使用连接池返回一个连接对象
	 * @return
	 * @throws SQLException
	 */
	public static Connection getConnection() throws SQLException {
		Connection con = tl.get();
		// 当con不等于null，说明已经调用过beginTransaction()，表示开启了事务！
		if(con != null) return con;
		return dataSource.getConnection();
	}

	/**
	 * 返回连接池对象！
	 * @return
	 */
	public static DataSource getDataSource() {
		return dataSource;
	}

	/**
	 * 开启事务
	 * 1. 获取一个Connection，设置它的setAutoComnmit(false)
	 * 2. 还要保证dao中使用的连接是我们刚刚创建的！
	 * --------------
	 * 1. 创建一个Connection，设置为手动提交
	 * 2. 把这个Connection给dao用！
	 * 3. 还要让commitTransaction或rollbackTransaction可以获取到！
	 * @throws SQLException
	 */
	public static void beginTransaction() throws SQLException {
		Connection con = tl.get();
		if(con != null) throw new SQLException("已经开启了事务，就不要重复开启了！");
		/*
		 * 1. 给con赋值！
		 * 2. 给con设置为手动提交！
		 */
		con = getConnection();//给con赋值，表示事务已经开始了
		con.setAutoCommit(false);

		tl.set(con);//把当前线程的连接保存起来！
	}

	/**
	 * 提交事务
	 * 1. 获取beginTransaction提供的Connection，然后调用commit方法
	 * @throws SQLException
	 */
	public static void commitTransaction() throws SQLException {
		Connection con = tl.get();//获取当前线程的专用连接
		if(con == null) throw new SQLException("还没有开启事务，不能提交！");
		/*
		 * 1. 直接使用con.commit()
		 */
		con.commit();
		con.close();
		// 把它设置为null，表示事务已经结束了！下次再去调用getConnection()返回的就不是con了
		tl.remove();//从tl中移除连接
	}

	/**
	 * 提交事务
	 * 1. 获取beginTransaction提供的Connection，然后调用rollback方法
	 * @throws SQLException
	 */
	public static void rollbackTransaction() throws SQLException {
		Connection con = tl.get();
		if(con == null) throw new SQLException("还没有开启事务，不能回滚！");
		/*
		 * 1. 直接使用con.rollback()
		 */
		con.rollback();
		con.close();
		tl.remove();
	}

	/**
	 * 释放连接　
	 * @param connection
	 * @throws SQLException
	 */
	public static void releaseConnection(Connection connection) throws SQLException {
		Connection con = tl.get();
		/*
		 * 判断它是不是事务专用，如果是，就不关闭！
		 * 如果不是事务专用，那么就要关闭！
		 */
		// 如果con == null，说明现在没有事务，那么connection一定不是事务专用的！
		if(con == null) connection.close();
		// 如果con != null，说明有事务，那么需要判断参数连接是否与con相等，若不等，
		//说明参数连接不是事务专用连接
		if(con != connection) connection.close();
	}
}
```

## **11.5、转账示例**

```java
public class AccountDao {
	public void updateBalance(String name, double balance) throws SQLException {
		String sql = "update account set balance=balance+? where name=?";
		Connection con = JdbcUtils.getConnection();
		QueryRunner qr = new QueryRunner();
		qr.update(con, sql, balance, name);
	}
}
```

```java
public class AccountService {
	private AccountDao dao = new AccountDao();

	public void transfer(String from, String to, double balance) {
		try {
			JdbcUtils.beginTranscation();
			dao.updateBalance(from, -balance);
			dao.updateBalance(to, balance);
			JdbcUtils.commitTransaction();
		} catch(Exception e) {
			try {
				JdbcUtils.rollbackTransaction();
			} catch (SQLException e1) {
				throw new RuntimeException(e);
			}
		}
	}
}
```

```java
AccountService as = new AccountService();
as.transfer("zs", "ls", 100);
```
