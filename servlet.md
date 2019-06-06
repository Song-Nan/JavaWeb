# servlet

#### servlet是什么？

servlet是一个运行在web服务器上，用于接收和响应客户端的http请求的一个Java程序，更多的是配合动态资源来做。当然静态资源也需要使用到servlet，只不过是tomcat已经定义好了一个defaultservlet。

#### servlet初次使用

1，得写一个web工程，要与一个服务器连接

2，测试该web工程

3，配置servlet     	用意：告诉服务器，我们的应用有这么写个servlet, 在webcontent/WEB-INF/web.xml里面可以写上以下内容

<!-- 向tomcat报告，我这个应用里面有这个servlet，名字叫做helloservlet，具体的路径是“包名+类名”

<servlet>	

​		<servlet-name>helloservlet</servlet>

​		<servlet-class>包名+类名</servlet-class>

</servlet>

<servlet-mapping>

​		<servlet-name>helloservlet</servlet-name> //servlet的映射，servletname: 

​		<url-pattern>/a</url-pattern>   //在地址栏上的path一定要以/打头

</servlet-mapping>



```java
<servlet>
    <servlet-name>helloservlet</servlet-name>
    <servlet-class>com.songjinzhao.servlet.helloservlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>helloservlet</servlet-name>
    <url-pattern>/a</url-pattern>
</servlet-mapping>
```

4，在地址栏上输入http://localhost:8080/项目名称 

#### servlet执行过程：

1，找到tomcat应用

2，找到项目

3，找到web.xml文件，然后在里面找到url-pattern,有没有哪一个pattern的内容是/a。

4，找到servlet-mapping元素中的那个servlet-name

5，在servlet元素里面找到servlet-name与步骤4相同的servlet-name,找到对应的servlet-class,然后开始创建该类的实例，

6，继而执行servlet中的service方法。

#### servlet通用写法：

​			servlet（接口）

​				|

​			Gerneralservlet

​				|

​			HttpServlet(用于处理http技术)

1，继承了HttpServlet类后，要写两个实现函数，doget 和dopost

#### servlet的生命周期：



**1.加载和实例化**

　　Servlet容器负责加载和实例化Servlet。当Servlet容器启动时，或者在容器检测到需要这个Servlet来响应第一个请求时，创建Servlet实例。当Servlet容器

启动后，它必须要知道所需的Servlet类在什么位置，Servlet容器可以从本地文件系统、远程文件系统或者其他的网络服务中通过类加载器加载Servlet类，

成功加载后，容器创建Servlet的实例。因为容器是通过Java的反射API来创建Servlet实例，调用的是Servlet的默认构造方法（即不带参数的构造方法），所

以我们在编写Servlet类的时候，不应该提供带参数的构造方法。





从创建到销毁的一段时间。

1，init方法：在创建该servlet实例的时候，执行该方法。一个实例只会初始化一次， init方法只会执行一次，默认情况下是：初次访问servlet，才会创建实例。。。

2，service方法：只要客户端有请求，那么就对这个方法进行执行，可以多次执行

3， destroy方法： 销毁的时候，就会执行该方法	

​	（1）该项目从tomcat的里面移除 	（2）正常关闭服务器

#### 让servlet创建实例的时间提前：

1，默认的时候，只有在初次访问servlet的时候，才会执行init方法，有时会做一些耗时的逻辑

2，有时候会在初始化的时候，逗留太久时间，有办法可以让init方法提前一点。

3，在web.xml配置servlet的时候用<load-on-startup>3</load-on-startup>来配置servlet的初始化实例对象的时间，一般数字越小，创建时间越早，一般不写负数。

#### ServletConfig:

servlet的配置，通过这个对象，可以获取servlet在配置的时候

1，得到servlet配置对象，专门用于在配置servlet的信息

2，可以得到具体的某一个参数

​		<init-param>name</init-param>

​		<init-param>value</init-param>

3,获取所有的参数名称

​	遍历取出所有的参数名称

#### 为什么要用servletConfig:

1，假如未来我们自己开发的一些应用，使用到了一些技术，或者一些代码，我们不会，但是有人写出来了，他的代码放在自己的servlet里面，

2，刚好这个servlet里面需要一个数字或者叫做变量，但是这个值不能固定了，所以要求用到这个servlet的公司，在注册servlet的时候，必须要在web.xml里面，声明init-params。



#### 总结：

###### http协议

1，使用http抓包看一看http背后的机制细节

2，基本了解请求和相应的数据内容

​		请求行，请求头，请求体

​		响应行，响应头 ，响应体

###### servlet

1，会使用简单的servlet

​		（1），写一个类，实现简单的servlet

​		（2），配置servlet

​		（3），会访问      

2，servlet 的生命周期

​	init 、 service 、destroy

3，servletConfig

获取配置的信息 

