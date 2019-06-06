# JDBC：

访问数据库流程：

加载数据库驱动--->建立与数据库的链接--->发送sql语句--->获得结果集

1，加载驱动类

Class.forName("com.mysql.jdbc.Driver")

2,建立连接（连接对象内部其实包含了Socket对象，是一个远程的连接，比较耗时，这是Connection对象管理的一个要点）真正开发中，为了提高效率，都会使用连接池来管理连接对象。

#### 测试sql语句(Statement)

Statement stmt = conn.createStatement();对于字符串

sql注入、对于字符串参数处理不方便、

#### prepareStatement:

可以做一些预处理，提高效率并且可以避免SQL注入。

步骤：

1，String sql = "select user_name ,user_password,user_age,user_desc from user_info where user_id > ?";   //占位符，需要时将参数传入。

2，prepareStatement ps = coon.prepaerStatment(sql);

3，preparedStatement.setInt(1, 3);//为占位符传入参数

4，rs = preparedStatement.executeQuery();//执行sql语句

5，返回结果集：

```java
while (rs.next()) {
    System.out.println(rs.getString(1) + "---" + rs.getString(2) + "---" + rs.getInt(3) + "---" + rs.getString(4));
}
```

6，调用完之后需要关闭所有的连接。从后至前依次关闭

```java
if (connection != null) {
    try {
        connection.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
```

#### 批处理：一次执行多条sql语句：

批处理的基本用法：对于大量的批处理，建议使用Statement，因为Prepare Statement的预编译空间有限。

```java
statement = connection.createStatement();
connection.setAutoCommit(false);
for (int i = 0 ; i < 20000 ; i++){
    statement.addBatch("insert into stu values('susan"+i+"',21,'man')");
}
statement.executeBatch();

connection.commit();
```

#### 事务：一组要么同时执行成功，要么同时失败的SQL语句。是数据库操作的一个基本单元。

事务起始于一个DML语句，结束于提交或回滚。

事务的四大特性：ACID

A：原子性：一个事务里面的所有操作都是一个整体，要么全成功，要么全失败。

C：一致性：事务里有个一个操作失败，所有其他的操作必须回滚到修改前的状态

I：隔离性：事务查看数据时数据所处的状态，要么是其他事物更改前的状态，要么是其他事物更改后的状态，事务不会查看中间状态的数据。

D：持久性：持久性事务完成之后，他对有系统的影响是永久的。

### rollback的使用：

首先将事务的自动提交改成false，然后再抛出异常的时候执行rollback语句。

#### JDBC时间的处理：

java.util.Data

	>子类：Java.sql.Date   表示年月日
	>
	>子类：Java.sql.Time 表示时分秒
	>
	>子类：java.sql.TimeStamp  表示年月日时分秒

## Clob:大对象//TODO

用于存储大量额文本数据，通常用流的方式存储。

怎么通过JDBC操作文本文件输入到数据库中？

## Blob:大对象//TODO





## 封装JDBCUtils工具类；

把对于数据库的每个操作可以封装成一个函数，以后调用方便，不用再写那么多无意义的代码，例如数据库的连接，数据库的断开，数据库名和密码的改变。

1，可以把对于数据库的所有操作重新写一个Java类，把数据库的连接和断开等写成静态方法，直接调用就好。

2，也可以将 1 升级操作，将数据库的各种信息放在新建的properties文件中，在JDBCUtiles里面调用，可以提升保密性。也可以用XML文件来存放数据库信息。

> E:\code\JDBC\JDBCUtils

在使用properties文件存放数据库信息的时候，可以定义下面的静态函数来获取数据库信息

```java
static{     //加载JDBC的时候调用一次
    pros = new Properties();
    try {
        pros.load(Thread.currentThread().getContextClassLoader().getResourceAsStream("db.properties"));
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

```java
public static Connection getMysqlCon() {
    Connection connection = null;
    Statement statement = null;
    ResultSet rs = null;
    try {
        Class.forName(pros.getProperty("mysqlDriver"));
        return DriverManager.getConnection(pros.getProperty("mysqlUrl"),
                pros.getProperty("mysqlUser"), pros.getProperty("mysqlPwd"));
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
        return null;
    } catch (SQLException e) {
        e.printStackTrace();
        return null;
    }
}
```

## ORM基本思想：object relationship mapping

表结构和类对应；表中的字段和类的属性对应，表中记录和类中的对象对应。

1，使用Object数组来封装一条记录；使用List <Object [ ] >封装多条记录

> E:\code\JDBC\SORM

List <Object[ ]> list = new Arraylist <Object [ ] >();

2,使用map来封装一条记录，然后用LIst来存放map。也可以用map来存放map。

3，用JavaBean对象封装一条记录

> 按照数据库中的表，建立Java类，一个表对应一个类，表中的字段对应类的属性，定义相应的构造器，和所有属性的    get    和    set   方法，和toString方法，当查询一条记录的时候先建立一个相应的类对象，再根据sql语句里面的字段 定义相应属性个数的构造器，用构造器new对应的对象，然后根据get方法输出。