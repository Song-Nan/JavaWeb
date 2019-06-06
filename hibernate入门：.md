# hibernate入门：

## CRM:（customer Relationship  Management)

客户关系管理	



Servlet  +  JSP  +  JavaBean + JDBC  使用这套架构可以开发市面上所有的应用，但是企业中不会使用（过于底层，开发效率太过低下），企业中开发一般使用ssh，（struts + Spring + Hibernate)、	SSM（SpringMvc + Sping + Mybatis),这是目前企业中常用的开发架构，

#### 什么是hibernate和什么是ORM？

//todo 百度百科

hibernate是一个持久层的orm框架，orm（object relationship management）：对象关系映射，指的是将一个Java中的对象和关系型数据库中的表建立一种映射关系，从而来操作数据库中的表，对象和表的映射通过xml文件来配置。

#### 为什么要学习hibernate？

与其他操作数据库的技术相比，hibernate有以下几个优点：

> hibernate对JDBC访问数据库的代码做了轻量级的封装，大大减少了数据访问层繁琐的重复的代码。并且减少了内存i下好，加快了运行效率。

>hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现，它很大程度上简化了DAO（数据访问对象）层的编码工作

> hibernate的性能非常好，映射的灵活性很出色，它支持很多关系型数据库，从一对一到多对多的各种复杂关系。

> 可扩展性很强，由于源代码的开源以及API的开放，当本身代码功能不够用的时候，可以自行编码进行扩展。

## hibernate的入门：

1，下载hibernate的开发环境（hibernate5）

2，解压，里面的三个目录

	>document：hibernate开发的文档
	>
	>lib:hibernate的开发包   ----》required 里面的Jar包是必须的，-----》optional  ：开发可选的jar包。
	>
	>project：hibernate提供的项目

##### 创建一个项目，引入jar包

hibernate可以在普通Java项目和web项目中使用

1，数据库驱动包，

2，hibernate开发的必须的jar包.

3，hibernate引入日志记录包

##### 创建实体类：//todo

##### *********创建映射：//todo 通过xml的配置文件完成，任意命名，尽量统一命名（类名.hbm.xml）。

xml里面类和表的映射建立

xml文件的约束在 引入的jar包里面的hibernate --- core...里面的 org.hibernate.  里面的configuration .dtd文件里

<clas1s name="类的全路径"  table="表名">	

​	<!-- 建立类中的属性与表中的主键对应-->

​	<id name="属性名"  column="主键名">

​		<generator class = "native"/>

​	<!--建立普通属性和表中的字段对应-->

​	<property name = "属性名"  column="字段名"/>

​	。。。。。。

​	主键用<id>其他属性用<property>

</class>

```java 
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <!--建立类与表的映射-->
    <class name="com.temperature.pojo.userInfo" table="user_info">
            <!--建立主键与属性的映射-->
        <id name = "userId" column="user_id"><generator class="native"></generator> </id>
        <property name="userAge" column="user_age"/>
        <property name="userName" column="user_name"/>
        <property name="userSex" column="user_sex"/>
        <property name="userDesc" column="user_desc"/>
        <property name="userImg" column="user_img"/>
        <property name="userPasswd" column="user_password"/>
        <property name="userUnionid" column="user_unionid"/>
        <property name="userTel" column="user_tel"/>

    </class>
</hibernate-mapping>
```

##### *************创建hibernate的核心配置文件：

###### hibernate的核心配置文件的名称：hibernate.cfg.xml

在src下新建一个hibernate核心配置文件

<hibernate-configuration>

​	<session-factory>

​		<!--链接数据库的基本参数-->

​		<property>name= "hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>

.......

​		<!--配置hibernate的方言-->

​	</session-factory>

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!--链接数据库的基本参数-->
        <propertyname="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql:///fehead</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">root</property>
        <!--配置hibernate的方言-->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <!--打印sql语句-->
        <property name="hibernate.show_sql">true</property>
        <property name="hibernate.format_sql">true</property>
        
        <!--告诉映射的哪个文件-->
        <mapping resource="com/temperature/pojo/user_info.hbm.xml"></mapping>
    </session-factory>
</hibernate-configuration>
```

***************8编写单元测试：

```Java
public class hibernate {
    @Test
    //  保存用户
    public void demo01(){
    //1，加载hibernate的核心配置文件
        Configuration configuration = new Configuration().configure();
    //2,创建一个SessionFactiory对象，类似于JDBC中的连接池
        SessionFactory sessionFactory = configuration.buildSessionFactory();
    //3,通过SessionFactory获取到session 类似于JDBC中的Connection
        Session session  = sessionFactory.openSession();
    //4,手动开启事务
        Transaction transaction = session.beginTransaction();
    //5,编写代码
        userInfo userInfo = new userInfo();
        userInfo.setUserName("王东");
        userInfo.setUserPasswd("123456");
        userInfo.setUserTel("13572572850");
        session.save(userInfo);
    //6，事务提交
        transaction.commit();;
     //7,资源释放
        session.close();
        }
```

# hibernate的常见配置：

### 1，映射的配置

**class**标签的配置 ：标签用来建立类和表的映射关系

属性：

		table：表名
		name ：类的全路径；
		catalog：数据库名		
**id**标签用来建立类中属性和表中的主键的对应关系

属性：name:	类中的属性

​		column:	表中的字段（类中的属性名如果和表中的字段名一致，则可以省略column）

​		length:	长度（hibernate可以根据映射建表，如果数据库中没有表，可以根据字段长度建表）

​		type:

property标签：用来建立类中普通属性与表中字段对应的关系

属性：name:		类中的属性

​		column:	表中的字段

​		length:	长度（hibernate可以根据映射建表，如果数据库中没有表，可以根据字段长度建表）

​		type:		类型，

​		not-null:	设置非空

​		unique: 	设置唯一	

## 2，核心配置文件的配置

1，必须的配置

​	*****包含链接数据库的基本参数：驱动类，url，用户名，密码

​	*****方言

2，可选的配置

​	*****显示sql语句	：hibernate.show_sql

​	*****格式化SQL语句	:hibernate.format_sql

​	*****自动建表	: hibernate.hbm2ddl.auto  :(none : 不使用hibernate的自动建表，create：如果数据库中已有表，则删除原有表，重新创建，如果没有，则新建，create—drop：如果数据库中已有表，则删除原有表，执行操作，删除这个表，如果没有表，新建一个使用完了删除该表  一般在在测试的时候使用；update：如果数据库中有表，则使用原有的表，如果没有表，则创建表，可以更新表结构；validate：校验：如果没有表，不会创建表，只能使用原有的表，可以映射和表结构是否一致，不一致就会报错 )

3，映射文件的引入

*****引入映射文件的位置

<mapping resource="映射文件的全路径"/>

## 3,hibernate的核心API

#### 1,Configuration：	hibernate 的配置对象，

​        Configuration 类的作用是对Hibernate 进行配置，以及对它进行启  动。在Hibernate 的启动过程中，Configuration 类的实例首先定位映射文档的位置，读取这些配置，然后创建一个SessionFactory对象。虽然Configuration 类在整个Hibernate 项目中只扮演着一个很小的角色，但它是启动hibernate 时所遇到的第一个对象。

​	*****1，加载核心配置文件	：

​		如果加载属性文件(hibernate.properties文件) 直接Configuration cnf = new Configuration();然后手动加载映射

cnf.addResource(”映射文件的全路径“)；

​		如果配置文件是xml文件：Configuration cnf = new Configuration().configure();

​	*****2，加载映射文件

### 2，SessionFactory:	session工厂

​        SessionFactory接口负责初始化Hibernate。它充当数据存储源的代理，并负责创建Session对象。这里用到了工厂模式。需要注意的是SessionFactory并不是轻量级的，因为一般情况下，一个项目通常只需要一个SessionFactory就够，当需要操作多个数据库时，可以为每个数据库指定一个SessionFactory。

*****内部维护了hibernate的连接池和hibernate的二级缓存（已被redis替代），是线程安全的对象，一个项目只用创建一个就好。

##### 抽取工具类：

## 3，Session（非常重要）类似JDBC中的Connection对象

​       [Session](https://baike.baidu.com/item/Session)接口负责执行被持久化对象的[CRUD](https://baike.baidu.com/item/CRUD)操作(CRUD的任务是完成与数据库的交流，包含了很多常见的SQL语句)。但需要注意的是[Session对象](https://baike.baidu.com/item/Session%E5%AF%B9%E8%B1%A1)是非[线程安全](https://baike.baidu.com/item/%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8)的。同时，Hibernate的[session](https://baike.baidu.com/item/session)不同于JSP应用中的HttpSession。这里当使用session这个术语时，其实指的是Hibernate中的session，而以后会将HttpSession对象称为用户session。

session代表的是hibernate与数据库的连接对象，不是线程安全的，与数据库交互的桥梁

#### *****Session中的API

##### 	1，保存方法：Serializable save(Object obj);

##### 	2，查询方法：T get（class c , Serializable id)

##### 				T load(Class c , Serializable id)

​		get方法和load方法的区别：get方法：采用的是立即加载，运行到这行代码的时候，就会马上发送sql语句去查询。查询后返回的是真是对象本身。查询一个找不到的对象的时候，返回null.

​		load方法：采用的是延迟加载，，执行到这行语句的时候，不会发送sql语句，当真正使用这个对象的时候才会发送SQL语句。返回的是代理对象。利用Javassist技术产生代理。查询一个找不到的时候，会返回异常。	

##### 	3，修改方法：

​		1，直接创建对象进行修改

​		2，先查询再修改

##### 	4，删除方法

​		1，直接创建删除

​		Customer   customer = new Customer;

​		customer.setId(1l);

​		session.delete(customer);

​		2，先查询在删除		

​		Cusstomer customer = session.get(Customer.class,1l);

​		session.delete(customer);

##### 	5,保存或更新

​		1，session.saveOrUpdate（）；有id就是更新，美欧id就是保存。

##### 	6，查询所有

​		1，Session.creaeteQuery("")

### Transaction对象

事务的对象，

​	1，commit  提交

​	2，rollback 回滚





# 持久化类的编写规则

主键的生成策略 <generate

什么是持久化类？	持久化类 = Java类   +  映射文件

持久化：将内存中的一个对象永久化的存到数据库的过程

持久化类：指的是一个Java对象与数据库的一个表建立了映射关系，那么这个类叫做持久化类，

持久化类的编写规则：

   **首先需要提供一个无参构造器，		：hibernate底层需要反射来生成实例

​     **属性需要私有，然后提供共有的get  set方法	：hibernate中获取，设置对象的值。

​     **持久化类里必须要有一个唯一的OID与数据库的主键对应，	：Java中通过对象的地址区分是否为同一个对象，数据库里根据主键确定是否为同一个记录，在hibernate里面通过持久化类的OID的属性判断是否为同一个对象。

​	**持久化类中的属性尽量使用包装类的类型定义。因为基本的数据类型默认值是零，会有很多的歧义。包装类的默认值是null。

​	**持久化类不要用final进行修饰，	：延迟加载本身是hibernate优化的一个设计。返回的是一个代理对象（通过Javassist技术实现， Javassist可以对没有实现接口的类产生代理--使用了非常底层的字节码增强技术，继承这个类进行代理如果不被继承，不能产生代理，那么延迟加载load和立即加载get方法就没有什么区别了。

### 主键的分类：

1，自然主键

​	指的是主键的本身就是表中的一个字段（可以是实体中的具体属性）

2，代理主键

​	主键的本身不是表中必须的一个字段，不是实体中的某个具体的	属性。

在实际的开发中尽量使用代理主键。	：一旦主键参与到业务逻辑层，后期就很有可能修改源代码。

好的程序设计满足OCP原则：  对程序的扩展是open的，对源代码的修改是close的，

### 主键的生成策略：

实际开发中一般不允许用户手动生成主键。在hibernate中，为了减少程序的编写，提供了多种的主键的生成策略。

​		increment  ：hibernate中提供自动增长机制，适用于short 、int  、 long类型的主键 ，多线程不适用。

​		indentity：适用于short ，int，long类型的椎间，使用的是数据库的自动增长，但是Oracle没有自动增长。

​		sequence：适用于short ,int,long类型的主键，采用的事序列的方式，Oracle支持序列，MySQL不能用sequence。 

​		uuid：适用于字符串类型主键，使用hibernate中随机生成字符串作为主键。

 		native：本地策略，可以在identdty和sequence之间自动切换。

​		assigned：hibernate放弃外键的管理，需要通过手动编写程序或者用户自己设置。

​		foreign：在一对一的一种关联映射的情况下使用。

### hibernate持久化类的三种状态：

hibernate是个持久层框架，通过持久化类来完成ORM操作，hibernate为了更好的管理持久化类，将持久化类分为三种状态。

1，瞬时态

​	这种对象没有唯一的标识，没有被session管理（调用session方法） ，称之为瞬时态对象，

2，持久态

有唯一的标识OID，被session管理，称之为持久态对象。持久态对象能自动跟新数据库，因为有一级缓存，

3，脱管态

有唯一的标识OID，但是没有被session管理，称之为脱管态对象。

##### 瞬时态对象：

​	获得：new 函数创建一个对象。

​	瞬时---》持久：调用 save函数或者saveOrUpdate函数

​	瞬时---》托管：customer.setCust_id(1);set个id；

##### 持久态对象：

​	获得：get(),   load().    find(),     iterate(),   等函数的调用

​	持久---》瞬时：调用delete（）函数。

​	持久---》托管：session.close();  sessoin.clear();

##### 脱管态0对象：

​	获得：先new一个对象，然后给它设置一个id，可以认为它是一个脱管态的对象。

​	托管---》持久：调用update（），或saveOrUpdate();

​	托管---》瞬时：setid(null); 设置OID为null.

### 持久态对象的特性：

##### 持久化类的持久态对象可以自动更新数据库：

//TODO





## hibernate的一级缓存：

什么是缓存：

​	是一种优化方式。把某些数据放在内存当中，使用的时候直接从缓存获取，不用通过存储源。

hibernate的缓存：

hibernate的一级缓存：称之为session级别的缓存，一级缓存的生命周期和session一致，一级缓存是自带的，不可卸载的。

hibernate的二级缓存是sessionfactory级别的需要配置的缓存。

​	证明一级缓存的存在。

//TODO

#### 一级缓存的内部结构：

###### 一级缓存中的特殊区域：快照区

session.clear();清空一级缓存		session.evict(user);清空该对象的一级缓存。

## hibernate的事务管理：

事务：	逻辑上的一组操作，组成这组操作的各个单元要么同时成功，要么同时失败。

特性：	原子性：事务不可分割；一致性：代表事执行前后数据完整性保持一致；隔离性：代表一个事务在执行的过程中不受其他事务的干扰；持久性：代表十五执行完成后，数据就持久到数据库中；

#### 如果不考虑隔离性，就会引发安全性的问题：

###### 读问题：脏读：	一个事务读到另一个事务还未提交的数据。

###### 		不可重复度：	一个事务度到另一个事务已经提交的update的数据，导致前一个事务多次查询结果不一致。

###### 		虚读：一个事务读到另一个事务已经提交的insert的数据，导致前一个事务多次查询数据不一致









#### 读问题的解决：

设置事务的隔离级别：

​	1，read uncommitted :以上问题都会发生

​	2，read committed	：解决脏读，但是不可重复读和虚读有可能发生

​	3，repeatable read	：解决脏读和不可重复读但是虚读有可能发生

​	4，serializable 	：解决所有读问题

### hibernate中设置事务隔离级别：

<property  name ="hibernate.connection.isolation">4</property>

### Service层事务

hibernate解决service层的事务管理

Service中封装业务逻辑操作，可能有很多持久层的操作，但是得保证每次操作的Session都是同一个，所以得使用ThreadLocal对象，将这个连接绑定到当前线程里，，在DAO方法中，通过当前线程获取到连接对象，

Hibernate框架内部已经绑定好了Thread Local，在SessionFactory中提供了getcurrentSession();但是这个方法默认不能用，得再核心配置文件里配置，然后调用。

1，改写工具类  hibernateUtils 

​	提供getCurrentSession(){};

2，配置文件 	<property name="hibernate.current_session_context_class">thread</property>

### HIbernate的其他API：

1，Query(重点掌握)

​	



2，Criteria（重点掌握）

条件查询

更加面向对象的查询

3，SQLQuery



开发中如果需要的查询非常复杂，例如要关联八九个表查询的话，可能需要 





方式一：再业务层获得Connection对象，传递给Dao。 





# Hibernate的一对多关联映射

## 1，数据库表与表的关系：

#### 1），一对多的关系：部门和职员、客户和联系人、

一对多建表原则：再多的一方创建外键指向一的一方的主键

#### 2），多对多的关系，一门课可以被多个学生能够选择，一个学生可以选择多门课程

多对多的建表原则：需要创建一个中间表，中间表至少需要两个外键指向多对多的双方的主键。

#### 3），一对一的关系，一个公司只能有一个注册地址，一个注册地址只能被一个公司使用。

一对一建表原则：1），唯一外键对应：把它当作一对多来处理。2），主键对应





#### 实例：

##### 1，创建数据表：

##### 2，创建实体对象，

##### 3，创建实体类的配置文件（多的一方）

，其中属性完了之后是多对一标签<many-to-one>标签，其中三则属性，name：一的一方的对象的属性名称。class：一的一方的类的全路径、column：在多的一方的表的外键的名称

创建配置文件（一的一方），在基本属性配置完之后配置，

<set name ="LinkMans">  name  :多的一方的对象集合的属性名称

​		<key column ="lkm_cust_id"/> key ：column：多的一方的外键的名称（和多的一方里面的外键名称为同一个。

​		<one-to-many class=""/>class：多的一方的类的全路径

</set> 

##### 4，创建核心配置文件	

需要映射多个映射配置文件：

session保存的时候必须两边都保存，不然会报错：持久态关联了瞬时态

### 一对多的级联操作：

什么是级联操作：指的是，我们操作一个对象的时候，是否会同时操作于其关联的对象，

级联是由方向性的：

​	1，操作一的一方时候会操作到多的一方，

​	2，操作多的一方时候会操作到一的一方

需要在操作的对象的配置文件里配置级联操作	

在一的一方配置的时候，在<set name="">后面加上级联配置<set name="" casecade="save-update">

在多的一方配置的时候，在<many-to-one name="">后面配置级联操作：<many-to-one name="" casecade="save-update" class="">

级联保存或更新：

保存

### 一对多对象的导航与测试：

前提：一对多的双方都设置了casecade=”save-update“

### 级联删除：删除一边的时候同时将另一边的删除一并删除

​	删除要先查询

在jdbc中如果有关连则不能删除，hibernate中则可以，hibernate首先将外键置为null，然后删除。

级联删除要首先在操作主体对象配置文件里配置：casecade="save-update,delete";

### Inverse:

一对多设置了双向的关联产生多与的SQL语句：

解决多与SQL语句：

​	1，单向维护

​	2，使一方放弃外键维护，是一的一方放弃，在set 上配置inverse=”true"

一对多的关联查询的修改的时候。要使用inverse

### casecade 控制的是是否能操作与之关联的对象，而inverse控制的是一的一方能否维护外键

# HIbernate的多对多的关联映射





























# HIbernate_Day04

## hibernate的查询方式：

在hibernate中，提供了很多种的查询方式，hibernate一共提供了5种查询方式：

#### 1，OID查询(了解)

​	指的是hibernate根据对象的OID（表中的主键）来进行检索

​		1，1，Session.get(customer.class,1);

​		1.2，Session.load(...)

#### 2，对象导航查询(了解)

​	指的是hibernate根据一个已经查询到的对象获得与其关联的对象的一种方式：

​	LinkMan linkman = session.get(LinkMan.class   ,   1);

​	Customer customer = linkman.getCustomer();

#### 3，HQL检索（重点）

（Hibernate Query  Language  面向对象的一种查询语言),语法类似于SQL，通过session.crateQuery（），用于接受HQL进行查询的一种方式。

##### 	3.1，初始化数据:

##### 	3.2 HQL的简单查询：

​		3.2.1sql里面支持*****的写法，但是在hql里面不支持*****的写法

​		3.2.2，别名查询		

​		Query query = session.createQuery("from Customer");

​		List<Customer>list = query.list();

##### 	3.3，HQL的排序查询：

​		默认是升序的，

​		session.createQuery("from Customer order by cus_id desc");降序查询。	

##### 	3.3，HQL的条件查询：

###### 		3.3.1，按位置绑定	（按参数的位置绑定）

​			where   

##### 		3.3.2，按名称绑定

​	Query query = session.createQuery("from Customer where cust_name = :aaa and cust_resource  =  :bbb");

query.setParameter("aaa","姓名");

query.setParameter("bbb","来源");

##### 	3.3，HQL的投影查询：（查询对象的某个或某些属性），要用到别名查询

​		单个属性：

​			List <Object> list = session.createQuery("sellect c.cust_name from Customer c").list();

​		多个属性：

​			List<Object []>list = session.creaetQuery("select c.cust_name,c.cust_resource    from Customer   c").list();

​		多个属性，将结果封装在对象中，要用到构造方法

​			List<Customer> list = session.createQuery("select new Customer(cust_name,cust_resource) from Customer").list();

##### 	3.3，HQL的分页查询：

​		Query query = session.createQuery("from Customer");

​		query.setFirstResult(0);

​		query.setMaxResult(10);

​		List <Customer> list = qurery.list()；

##### 	3.3HQL的分组统计查询：

​		Query.query = session.createQuery("select count(*) from Customer").uniqueResult();

#### 4，QBC检索（重点）

Query By Criteria  更加面向对象化的查询方式，并不是所有的都可以用：

##### 	4.1，简单查询：

​		Criteria criteria = session.createCriteria(Customer.class);

​		List<Customer> list = criteria.list();

##### 	4.2  排序查询

​		Criteria  criteria = session.createCriteria(Customer.class);

​		criteria.addOrder(Order.asc("cust_id"));//升序

​		criteria.addOrder(Order.desc("cust_id"));//降序

​		List <Cutomer>list = criteria.list();

##### 	4.3 分页查询

​		同HQL写法相似，将Query换成Criteria

##### 	4.4 条件查询

​		Criteria criteria  =  session.createCriteria(Customer.class);

​		criteria.add(Restrictions.eq("cust_source","小广告"));

​		List<customer>list = criteria.list();

​		条件：	1:   =   eq;     2:  >   :    gt;    3: >=  ge; 	4:  <   lt

​				5:  <=  le;   6 : <>   ne;  like  and   in   or   

##### 	4.5，统计查询

​		criteria.add 	:普通的条件，where后面跟条件

​				.addOrder	:排序查询

​				.setProjection	:聚合函数和group by having  

​		criteria.setPorjection(Projection.rowCount());

​		Long num = (Long) criteria.uniqueResult();

##### 	4.6,离线条件查询//TODO

#### HQL多表查询

##### 	SQL的多表查询

###### 		连接查询：内连接：inner   join ，查到的是两个表的交集 ，外连接，交叉连接（笛卡儿积）

###### 		内连接：隐式内连接：在SQL语句中看不到inner  join  但是查询结果是一样的	：select * from A ,B where  A.id = B.aid;

​				显示内连接：可以看到inner join 	select * from A inner join  B  on A.id = B.aid; 

###### 		外连接：

###### 		左外连接：left outer join (outer 可以省略) 左表全部信息和 和右表的交集

​		select * from  A left  outer join B on A.id = B.aid;

###### 		右外连接：right outer join(outer 可以省略)右表所有信息和和左表的交集

​		select * from A right outer join B on A.id = B.aid;

#### HQL多表查询：

​	外连接：左外连接，右外连接，迫切左外连接，

​	内连接：显示内连接，隐式内连接，迫切内连接

​	内连接：session.createQuery("from  Customer  c inner  join c.linkMans").list();

##### 	迫切内连接：在普通内连接后添加一个关键字fetch；

​	session.createQuery("from customer c  inner  join fetch  c.linkMans").list();  //fetch  通知hibernate将另一个对象的数据封装在前一个对象的

​	List<Customer>list = session.createQuery("select distinct c from customer c inner join  fetch c.LinkMans");

##### 5，SQL检索(通过使用基本的SQL语句进行查询)

SQLQuery sqlQuery = session.createSQLQuery 	("select* from customer");

sqlQuery.addEntity(Customer.class);

List <Customer>list = sqlQuery.list();



## Hibernate的抓取策略

（优化手段）

延迟加载：在hibernate中叫做lazy（懒加载）。执行到改行代码的时候不会发送语句区进行查询，在真正使用这个对象的属性的时候才会发送SQL语句区查询。

延迟加载的分类：

​	类级别的延迟加载：

​	指的是通过load方法查询某个对象的时候，是否采用延迟加载。

​	关联级别的延迟加载：

​	指的是在查询某个对象的时候，查询其关联的对象的时候是否采用延迟加载。（通过客户对象区获取联系人的时候，联系人是否采用了延迟加载)

​	类级延迟加载：就是在《class标签》上配置lazy，class上设置的lazy属性只对普通属性有效，对关联属性无效，必须在关联属性的<set >标签上设置或者<many-to-one>标签上重新设置

​	抓取策略往往会和关联级别的延迟加载一起使用，优化语句。

2，抓取策略（通过一个对象抓取到关联对象需要发送SQL语句，SQL语句如何发送，发送成什么格式通过策略配置）

​	通过<set>或者<many-to-one>上通过fetch属性进行设置。

​	通过fetch和lazy如何设置优化发送的SQL语句。

​	<set>上的fetch和lazy：

​		fetch：抓取策略，控制SQL语句格式，取值：select (默认）：发送普通select语句，查询关联对象；join：发送一条迫切左外连接查询关联对象subselect：发送一条子查询查询其关联对象。

​		lazy：延迟加载，控制查询关联查询对象的时候是否采用延迟。取值：true 就（默认，采用延迟加载）； false ：不采用关联加载；  extra：及其懒惰。

​	在实际开发中，一般都采用默认值，如果有特殊需求，eg:每次查客户都需要查看他的联系人，需要配置join，只需和数据库加护一次，其他的都需要交互两侧。  

#### 3，在<many-to-one>上配置fetch和lazy

​	fetch：抓取策略：取值：select：（默认）发送一般查询语句查询关联对象；join：发送一条迫切左外连接。

lazy：延迟加载：取值：false：不延迟；proxy：使用代理，检索订单额时候，是否马上检索客户信息，有


　　　　false	:不延迟

​		proxy	:使用代理.检索订单额时候,是否马上检索客户 由Customer对象的映射文件中<class>上lazy属性来决定.

　　　　no-proxy	:不使用代理（不研究）

## 4，批量抓取：

什么是批量抓取：一批关联对象一起抓取，batch-size

测试批量抓取：

在c