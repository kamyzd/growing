说到JDBC，你知道他的全称吗？Java Data Base connectivity (java数据库连接)。java 针对数据库的操作，最初就是通过这种方式，在我大学的时候，也是使用这种方式连接，已哭瞎，当时考虑的东西太少，只注重实现功能。有了JDBC API ，java 程序猿就可以通过其连接到数据库，并使用结构化查询语言 SQL（Structured Query Language）完成对数据库的查找与更新。但是，在毕业以后接触到真正的项目中，发现使用的不会再有这种方式，那么是使用什么方式呢？请听我一一道来。

#java JDBC API
JDBC的设计是参考了微软的ODBC，那么ODBC是一套 C语言访问数据的的API。JDBC 和 ODBC 都基于同一个思想：根据其 API 编写的程序都可以与**驱动管理器**进行通信，然后**驱动管理器**则通过**驱动程序**与**实际的数据库**进行通信。那么**驱动程序**扮演着很重要的角色。JDBC规范将驱动程序归结为以下几类：

##第1类驱动程序：将JDBC 翻译成 ODBC,然后使用 ODBC 驱动程序与数据进行通信。

##第2类驱动程序：由部分 Java 程序和部分本地代码组成，用于与数据库的客户端API进行通信。这种驱动程序，比较繁琐的地方是：客户端不仅需要安装 Java 类库，还需要安装一些与平台相关的代码。

##第3类驱动程序：纯 Java 客户端类库，其使用一种与数据库无关的协议将数据库请求发送给服务器构件，然后该构件再将数据库请求翻译成数据库相关的协议。这种方式，将简化了部署，将平台相关的代码都放到了服务器端。

##第4类驱动程序：也是纯 Java 客户端类库，但是，它是将 JDBC 请求直接翻译成数据库相关的的协议。

驱动程序一般有数据库供应商提供，而大多数的数据库供应商提供第3类或低4类的驱动程序。但是也有第三方公司专门开发了很多符合标准的驱动程序，他们支持更多的平台、运行的性能也更加，某些情况下可靠性也更高。

接下来需要知道的是 JDBC 配置，那么你可能需要这些东东：

**数据库的url**一般为：主机名称、端口号、数据库名称。

**驱动程序jar包**：如果你用的oracle的数据库，那么你需要oracle相应的驱动jar；同理如果你使用的是mysql的数据库，那么你需要一个mysql相应的jar包，度娘会告诉你有一堆各式各样的jar在等着你。

**注册驱动器类**有如下两种方式：
1）Class.forName("org.postgresql.Driver");这条语句将使得驱动器类被加载，由此执行可以注册驱动器的静态初始化器。
2）设置jdbc.drivers 属性。可以用命令行参数来制定这个属性。如下：

    java -Djdbc.drivers=org.postgresql.Driver  项目名称

最后，你该干嘛？当然是链接数据库呗。

```
//获取数据库连接
String url = "jdbc:mysql:lcolhost:3306/test";
String username = "testUser";
String password = "123456";
Connecttion conn = DriverManager.getConnection(url,username,password);


//在获取数据连接以后，你就可以通过SQL实现数据的查找和更新
Statement stat = conn.createStatement();

stat.executeUpdate("CREATE TABLE Greeting (Message CHAR(20))");//程序到这，在数据库中就成功创建了这张表。

stat.executeUpdate("INSERT INTO Greeting VALUES('hello,World!')");//向数据库中插入一条数据

ResultSet result = stat.executeUpdate("SELECT * FROM Greeting");//查询数据库

System.out.println(result.getString(1));//打印查询结果

```
    
#java jdbcTemplate
