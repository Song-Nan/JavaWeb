# Struts2

##### 概述：

###### 什么是struts2？

基于mvc设计模式的web应用框架，本质上是一个servlet。相对于struts1来说已经发生了巨大的变化，可以说是两个不同的框架。

###### 常见的web层框架：

`struts2`，`struts1`，`webwrok`，`springmvc`，

web层框架都是基于前端控制器模型设计的。

![1554166381885](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1554166381885.png)

##### struts2入门：

下载解压struts2包；

目录：

apps：struts2提供的应用，war包，Java应用打成jar包，web项目打成war包；

docs：struts2开发的文档和api；

lib：struts2开发的jar包；

src：struts2的源码；

##### 创建：（必须是web项目）

引入jar包

创建jsp页面，

创建class以及类里面的方法，方法名称固定：public String execute（）{}；

再web.xml中配置。

创建的过程中出现了这个错误：

Error during artifact deployment. See server log for details

非常莫名其妙！！！！

更加莫名其妙的是：我改了  ![1554173987013](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1554173987013.png)

![1554173997374](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1554173997374.png)

里面的problems用fix修改了一下，错误突然就都没了！！！！没了！！！了！！！

莫名其妙！

struts2的常量配置，常量配置在（default.properties)里面。如果想要修改常量的值，可以再struts.xml里面修改，或者再strut.propertiey里面修改，或者再web.xml里面修改。

struts.xml中修改：一般修改常量再文档的最上方，用<constant>标签。<constant name="常量原名"  value="变量名"/>

<constant name="struts.action.extension"   value="abc"/>

struts.properties中修改：再src下新建properties文件，只能修改常量，不能配置action。

在web.xml中修改常量：在<filter>标签里面通过

<init-param>

​	<param-name>...</>

​	<param-value>...</>

如果三个文件都修改了，最后又web.xml生效，根文档的加载顺序有关。

大家都习惯在struts.xml中修改。

## include的配置：在分模块开发的时候常用：

在struts.xml中使用，主要用于引入其他的配置文件，分模块开发时使用。

## Action的访问

#### Action的编写：（推荐使用第三种继承ActionSupport类的方法）

Action的类就是一个POJO的类

Action的类就是一个实现Action接口的类

Action就是一个继承ActionSupport类

访问：（3种情况）

1，通过method设置

2，通过通配符的方式进行设置

3，动态方法访问

# day_2

## struts2的serveletAPI的访问：

### 1，使用完全解耦和的方式：

//TODO

只能操作域对象中的数据，不能获的对象的方法。

### 2,使用原生的方式访问Servlet的API：

HttpServetRequest    request	= 	ServletActionContext.getRequest(  ) ;

得到request和responce，来使用request的方法。

第二种方式可以操作域中的数据，同时也可以获得对象的方法。

### 3，接口注入的方式：

（使用Action类实现一些接口，通过接口的方法来实现具体的方法）

提供成员变量的话，不会出现线程安全问题。

### struts2的结果页面设置：

页面跳转的时候采用的是转发。如果想要重定向的话要用result配置

1，全局结果页面：指的是在包中配置一次，其他的在在包中所有的action只要返回这个值，就都可以跳转到这个页面。

​	配置：<global-result> 

​				<result>........

​			</global-result>

在package标签里配置。

2，局部结果页面：指的是，只能在当前的action中的配置有效。

#### 采用就近原则。

## result标签的配置：

用于配置页面跳转，

#### name：逻辑视图名称，默认值：success。

#### type：页面跳转的类型 ；

###### dispatcher：转发   Action转发JSP

######  redirect：重定向   Action重定向到JSP

chain：转发   Action转发到Action

redirectAction：重定向	Action重定向到另一个Action

stream：struts2中提供了文件的下载功能，

# 数据封装

### 1，属性驱动：

#### 1），提供set方法的方式：

​	首先提供对应的属性以及属性对应的set方法。表单中的属性名称

#### 2），页面中提供表达式方式：

表单中的属性和Action中类的属性的名称一样。

### 2，模型驱动：（应用最多的）

#### 采用模型驱动方式

1，编写jsp页面。

2，建立Action的时候要实现ModleDriven的接口。





# 复杂数据类型的封装：

在实际开发中，有可能遇到批量象数据库中插入记录，需要在页面中将数据封装到集合中。

封装数据到List集合中：

​	

封装数据到map集合中：			

# OGNL的概述：

### 什么是OGNL？

对象图导航语言，表达式语言，比EL强大很多倍。

EL：只能从域对象中获取对象，从EL的11个对象中获取

OGNL：调用对象的方法，获取struts2的值栈的数据，类型转换等功能。是第三方的表达式语言。（没有struts2也可以用）

### 为什么学习OGNL？

1，支持对象方法调用，如xxx.doSomeSpecial();

2，支持静态方法的调用和值访问，

##### 3，访问OGNL上下文；

4，操作集合对象等。。。。。

### OGNL使用的要素：

1，表达式。2，根对象（操作谁）。3，context对象。（在哪操作）

### OGNL的入门：（Java环境，了解）：

#### OGNL调用对象的方法：

public void  demo1()  throws  OgnlException{

OgnlContext 	context 	= 	new OgnlContext();

Object object = context.getRoot();

Object obj =  Ognl.getValue("   '   helloworld  ' .length()  "  ),  context,   root);

system.out.pritnln(obj);

}

#### OGNL调用对象的静态方法：

@类名@方法名





##  OGNL的struts2的入门：（主要在页面使用）

在jsp页面：<%@taglib uri="/struts-tags" prefix="s"%>

1，访问对象的方法，

```
<s:property value="'struts'.length()"/>
```

2，访问对象的静态方法

```
<constant name="struts.ognl.allowStaticMethodAccess" value="true"></constant>//struts2默认将访问静态方法关闭，需要手动打开
```

```
<s:property value="@java.lang.Math@random()"/>
```

## Value Stack （值栈）

#### OGNL最主要的应用就是获取值栈的数据：

### 值栈的概述：

### 什么是值栈？

ValueStack其实就类似于一个数据中转站，（struts2的框架中的数据都保存到了ValueStack中）；ValueStack接口；实现类OgnlValueStack对象；ValueStack贯穿了整个Action生命周期，Action一旦创建了，struts2框架就会创建ValueStack对象。

#### 1，值栈的内部结构：

ValueStack主要由两个部分组成：root和context

root：CompoundRoot:Arraylit的实现类。

context：ognlContext:	是一个Map 里面放的是web开发常用的对象数据类型的引用。request，session，application，parameter是，attr。获取root引用的时候不用加#，但是获取其他的要加#；

操作值栈，指的是操作root区。

可以在页面跳转的时候查看两个区域：<s:debug></s>

#### 2，值栈与ActonContext的关系

ServletContext：Servlet的上下文。Servlet的前和后都知道

ActionContext： 当请求过来的时候，执行过滤器中的doFilter方法，在这个方法中创建ActionContext对象，同时创建了Value Stack对象，将ValueStack对象传给ActionContext对象，所以可以通过ActionContext对象访问值栈，

ActionContext之所以可以获取Servlt的API(访问的是域对象的数据),因为在其内部有值栈的引用，

#### 3，获得值栈：

1，通过ActionContext对象获取值栈

ValueStack valuestack = ActionContext.getContext().getValueStack();

2，在struts2的内部，将值栈存放在request中一份，

ValueStack valuestack2 = (ValueStack)ServletActionContext.getRequest.getAttribute(ServletActionContext.STRUTS_VALUESTACK_KEY);

两种方式获得的值栈是同一个，一个Action的实例，只会创建一个值栈对象。

#### 4，操作值栈：—向值栈中存入数据

1，在Action中提供属性的Get方法

```
private User user ;

public User getUser() {
    return user;
}

public String execute(){
    user = new User("宋进钊","dell");
    return SUCCESS;
}
```

2，使用ValueStack中本身的方法的方法：push(Object obj);	set(String key,Object obj);

set 方法和push方法都是往栈顶放数据，set方法是创建了一个map集合，将这个集合放在栈顶

#### 5，获取值栈数据：

在页面使用OGNL表达式就可以了。

1，获取root的数据：不需要加#

ActionContext

2，获取Context的数据：

#### 6，EL为何获得值栈数据：



## OGNL中的特殊字符：

#### 1，#

1，获取context的数据；2，使用#构建一个map集合

获得context中的数据
<s:property value="#request.name"/>
<s:property value="#session.name"/>
<s:property value="#application.name"/>
<s:property value="#attr.name"/>
<s:property value="#parameters.id"/>
<s:property value="#parameters.name"/>
构建一个map集合
<%-- list --%>
<s:radio name="sex" list="{'男','女'}"></s:radio>
<%-- map --%>
<s:radio name="sex" list="#{'0':'男','1':'女'}"></s:radio>

#### 2，%

强制字符串解析成OGNL表达式。
例如：在request域中存入值，然后在文本框（<s:textfield>）中取值，写在value里。

<s:textfield value="%{#request.msg}"/>
{ }中值用引号引起来,此时不再是ognl表达式,而是普通的字符串，到底使用单引号还是双引号是由外层引号决定的。
<s:property value="%{'#request.msg'}"/>



#### 3，￥

## 数据校验：

1，手动编写方式：

1），继承ActionSupport类

2），重写validate方法：在这个方法中，编写校验的代码，针对Action里面的所有方法，

3），编写validateExecute（）方法：针对某一个具体的方法：要校验的方法首字母大写。

2，采用配置文件的方式







## 拦截器

什么是拦截器？Intercepter;

起到的是拦截action的作用，

Filter：过滤器，从客户端向客户端发送的请求，

Interceptor：拦截器，拦截的是客户端对Action的访问。更细粒度的拦截。可以拦截到Action中具体的方法。

struts2框架的核心功能都是依赖拦截器实现的

Struts2的执行流程：

客户端向服务器发送一个Action请求，执行核心过滤器（doFilter）方法，在这个方法中，调用executeAction（），在这个方法的内部调用了dispatcher.serviceAction();在这个方法当中创建了一个Action代理，最终执行的是Action代理中的execute（），在execute（）方法中调用了ActionInvotion的invoke帆发，在这个方法中递归调用了一组拦截器，（完成部分功能），如果没有下一个拦截器，就会最终执行目标Action，根据Action的返回的结果进行页面跳转。



## 拦截器入门：

编写拦截器：继承AbstractInterpetcor类。

对拦截器进行配置：

1，定义拦截器进行配置

2，定义拦截器栈进行配置

## struts2的标签库：

##### 通用标签库：

判断标签：《s:if》 《s:elseif》 《s:else》

  

UI标签库：

方便数据回显。