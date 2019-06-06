# Spring：

## Spring4学习路线：主要有IOC和AOP

Spring的概述：Spring是一个EE开发的一站式框架。

一站式框架：有EE开发的每一层的解决方案

​	WEB层：SpringMVC

​	Service层：Spring的Bean管理，Spring声明式事务

​	DAO层：Spring的jdbc模板，Spring的ORM模块。

## Spring的入门：（IOC入门）

IOC：（Inversion of Control)控制反转，是一个重要的面向对象编程的法则

控制反转：将对象的创建权反转（交给）给Spring，

##### 1，下载Spring的开发包

##### 2，解压Spring的开发包

目录：1），docs：Spring开发的规范和API　；　２），libs：Spring开发所需的jar包和源码；3），schema：Spring开发配置文件的约束。

##### 3，创建web项目，引入jar包，引入Spring开发核心容器里面的四个模块的包。beans,context,core,expression

##### 4，创建接口和实现类。

##### Spring的IOC底层实现：

<font color="red">传统方式：面向接口开发，（接口和实现类有耦合），切换底层的实现类，就得修改源代码。好的程序设计得满足OCP原则</font>

工厂模式：把所有类的创建都交给工厂。工厂  +   反射 +  配置文件  实现程序解耦合。

5，将实现类交给Spring管理

在Spring的解压路径下Spring-framework-4.2.4.RELEASE\docs\spring-framwork-refrence\html\xsd-configuration.html

然后配置Spring





# IDC和DI

IOC：控制反转，将对象的创建权反转给了Spring

DI：依赖注入，指的是前提是必须有IOC环境，Spring在管理这个类的时候会将这个类的属性注入（设置）进来。

DI：Spring在创建实例的过程中，在配置中取到属性设置在实例中，

面向对象的时候：

​	依赖：指的是两个类之间有调用关系，

​	继承：

​	聚合：松散的，紧密的，

## Spring的工厂类的结构图：

ApplicationContext继承了BeanFactory

#### <font color="red">BeanFactory	:老版本的工厂类：</font>

​	调用getBean的时候才会生成类的实例。

#### <font color="red">ApplicationContext	：新版本的工厂类</font>

​	在加载配置文件的时候就生成了类的实例

由两个实现类：

​	ClassPathXmlApplicationContext	:加载类路径下的配置文件----src下

​	FileSystemXmlApplicationContext	:加载文件系统下的配置文件

## Spring的响应的配置：

#### XML提示的配置：

​	schema的配置：

#### Bean的生命周期的配置:

<bean  id=""  name="">

id	:使用了约束中的唯一约束。里面不能出现特殊字符。

name	:没有使用约束中的唯一约束（理论上可以出现重复的，但是实际开发中不允许） ，可以出现特殊字符。

class	:要生成实例的类的全路径。

init-method	:Bean被初始化的时候执行的方法；

destroy-method	:Bean被销毁的时候执行的方法（Bean是单利创建，工厂关闭）。

## Bean的作用范围的配置（重点）

Scope	:Bean的作用范围

​	singleton	：默认的，Spring会采用单例模式创建对象

​	prototype	：多例模式  spring和struts2整合的时候用多例。

​	request	：应用在web项目中，spring创建这个类后将这个类存入到request范围i中

​	session	：应用在web项目中，Spring创建这个类后将这个类存入到session的范围中

​	globalsession	: 应用在web项目中，必须在porlet（有子网站的情况）环境中使用

## Spring属性的注入：

1，构造方法方式：

​	在applicationContext.xml里面配置：

<bean id = ""  class = "">

<constructor-arg name = "name"  value="lisa">

<constructor-arg name="age" value="21"> 

</bean>

2，set方法方式：

3，接口注入的方式：

### Spring的分模块开发的配置：

1，在加载配置文件的时候加载多个，

2，在一个配置文件里引用多个   <import  resourse="xxxxxxx.xml".





## SpringIOC的注解开发(重点)

#### SpringIOC注解开发入门

创建web项目

引入jar包和aopjar包，在web  4.xx另外引入aopjar包。

编写applicationContext.xml文件，引入约束条件。

1. 注解开发，需要引入context约束 ，在Spring框架下，xsd.configruation.xml里卖弄。

编写类，在xml文件里配置要扫描的包

#### 注解方式来设置属性的值：可以没有set方法 

属性如果有set方法，需要将属性注入的注解添加到set方法中

属性如果没有set方法，需要将属性注入的注解添加到属性上

#### Spring的IOC的注解的详解

@component:组件

修饰一个类：将这个类交给Spring管理

这个注解衍生出三个注解（功能相似）

​	@Controller：web层

​	@Service

​	@Resprsitory

## SpringAOP的XML的开发（重点） 