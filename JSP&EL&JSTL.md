# JSP&EL&JSTL

### JSP: java server page

从用户角度来看其实也是一个网页，从程序来看，是一个Java类，继承了servlet的类。

### 为什么会要有jsp?

> html多数情况下用来显示静态内容，一成不变的，但是有时候我们需要在网页上显示一些动态数据，比如：查询所有的学生的信息，这些操作都是要区查询数据库的，然后在网页上显示，html是不支持Java代码的，jsp里面可以写Java代码。

怎么用JSP?

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page 属性=“” 属性=“” 属性=“” 。。。。%>
```

> #### page指令：<%@ 指令名字
>
> language：表明jsp页面中可以写什么语言的代码;   声明jsp要被转译的语言
>
> contentType：告诉浏览器该文件是什么类型，
>
> pagencoding: jsp内容编码
>
> extends：用于指定jsp翻译成Java后继承的父类
>
>   import：导包，一般不用手写，快捷键就可以。import=“java.util.*  ,  java.lang.*"  
>
> session：只能油两个值：true  false  ，用于控制在该jsp界面能否直接使用session这个对象
>
> errpage：指的是错误的页面，值需要给出错误的页面路径
>
> iserrorpage：用于声明该页面是不是一个错误的页面

> #### include指令：

静态指令：在页面转换的时候执行的。

后台实现：把另外一个页面的代码拿过来一起输出。所有的标签元素都包含进来。

> #### taglib指令：

<%@ taglib prefix="" uri="">

  

### jsp的局部代码块：

局部代码块中声明的Java代码会被转移到jsp对应的servlet中的_JspService方法中去，（局部变量）代码块中声明的变量都是局部变量，内置对象在jsp页面中使用需要使用局部代码块或者脚本段语句来使用，不能够在全局代码块中使用。	

使用：<%	java 代码 %>

缺点：书写麻烦，阅读困难

在开发中使用：一般用servlet进行逻辑判断，用jsp进行页面展示

### jsp的全局代码块：

<%!   java 代码   %>

声明的Java代码作为全局代码被转移到对应的servlet中

在全局代码块中声明，在局部代码块中调用。

### jsp的脚本段语句:

特点：帮助我们快速的获取变量或者方法的返回值作为数据给浏览器。

使用：<%=变量名/方法名  %>  不能加分号。

可以写在jsp页面的任意位置。	

### 静态引入：

<%@ include file="文件的相对路径"%>

静态引入tomcat会把当前请求转译的和引入的jsp合并成一个servlet。

注意：静态引入的jsp文件不会单独转译成Java文件，直接访问会被转换。当前文件和静态引入的文件不能使用Java代码块声明同名变量。

### 动态引入：(include)

<%: include page="文件的相对路径"%>

会将引入的jsp文件按单独转译，在当前转译好的Java文件就按中调用引入的jsp文件转移的文件，并在网页中显示合并后的显示效果。

可以有同名变量。

### jsp的页面转发（forward):

<jsp:forward page="err.jsp">	

### 		<jsp: param value = " "  name = "" />

</jsp : forward>



### jsp的动作标签：

在<body></body>里面用： 

```
<jsp:include page="err.jsp"></jsp:include>
<jsp:forward page="err.jsp"></jsp:forward>
```

<jsp:include>:  包含指定的页面，这里是动态包含，也就是不把包含的页面的所有元素变迁全部拿过来输出，而是把它的运行结果拿过来。

jsp:forward：前往哪个页面。等同于一下代码：

​		<%		request.getRequestDispatcher("other.jsp").forward(resquest.responce);

<jsp:param :意思是在包含某个页面的时候，或者在跳转某个页面的时候



### jsp内置对象：

> 所谓内置对象，就是我们可以在jsp页面中直接使用，不用创建  ;   jsp转译成servlet的时候自动生成的对象，不能改变名称。
>
> pageContext ：页面上下文对象，封存了其他内置对象。封存了当前jsp的运行信息
>
> pageContext的作用域仅限于当前的页面，每个jsp页面单独拥有一个pageContext对象。
>
> request：作用域仅限于一次请求，只要服务器对该请求做出响应，这个域中存在的值就没有了，作用域一次请求。
>
> session：此对象用来存储用户的不同请求的共享数据的，一次会话。
>
> application：也就是ServletContext对象，一个项目只有一个，存储用户共享数据对象，以及完成其他操作。
>
> response：相应对象，用来响应请求处理结果给历览器对象，设置响应头，重定向。
>
> out
>
> page
>
> exception
>
> config：主要用来获取web.xml中的配置文件，完成一些初始化数据的读取。	 
>
> 整个工程都能访问，服务器关闭后就不能访问了  
>
> ### 四个作用域对象：

pageContext 当前页面有效时间最短（页面执行期）当前jsp，解决了当前页面的数据共享问题。获取其他内置对象。

 request HTTP请求开始到结束这段时间  ； 一次请求，一次请求的servlet的数据共享。通过请求转发，将数据流转给下一个servlet。

 session HTTP会话开始到结束这段时间；  一次会话，一个用户的不同请求的数据共享。将数据从一次请求流转给其他请求。前提是得是一个用户。

application 服务器启动到停止这段时间   项目内。不同用户的数据共享问题。将数据从一个用户流转给其他用户。

##### 共同作用：数据流转

>
>以上四个是作用域对象：表示这些对象可以存值他们的取值范围有限定。通过setAttribute   和 getAttribute 方法
>
>可以用来存值，也可以用来取值：
>
>pageContext.setAttribute("name","page");
>
>...........
>





> config
>
> page
>
> exception



> responce   [httpServletResponce]
>
> out            []
>
> responce和out

### jsp的路径问题：

在jsp中，资源路径可以使用相对路径完成跳转，但是有以下问题：

​	1，资源的位置不可随意更改。	

​	2，需要使用../进行文件夹的跳出，使用麻烦。

使用绝对路径：

​	/虚拟项目名/项目资源路径

​	<a href = " /jsp/jsppro.jsp">jsppro.jsp </ a> 

​	<a href= " /jsp/a/a.jsp">a.jsp</ a>

​	1，在jsp中资源的第一个/表示的是服务器根目录，相当于：localhost:8080

使用jsp自带的全局路径声明：

​	

## EL表达式：

是为了简化jsp代码，为了简化jsp中的Java代码。

写法格式：${ 表达式}

如何使用：

#### 1，取出四个作用域中的值：

${pageScope.name}

${requestScope.name}

${sessionScope.name}

${applicationScope.name}

如果这份值是有下标的，那么直接使用  [  ] ;

如果没有下表，直接使用    .   的方式来调用。

#### 2，一般使用比较多的都是从一个对象中去除它的属性



#### EL表达式的11个内置对象：

pageContext  、

pageScope、requestsScope 、 sessionScope、applicationScope

header  、 headerValues

param  、 params

cookie  、initParam



 



# JSTL：

全称：jsp standard tag library ： jsp  标准标签库，一般用来简化jsp代码，

使用：1，导入JSTL支持的jar文件，jstl.jar 和 standard.jar

​		2,在页面使用taglib来引入标签库。注意：如果想要支持EL表达式，引入的版本必须是1.1版本。

可以用来遍历。

#### 常用标签属性：

<c:set>

```java
<c:set></c:set>
<c:set var="name" value="zhangsan"></c:set>
<c:set var="name" value="zhangsan" scope="session"></c:set>
```

可以把数据直接存到page域中。可以用EL表达式直接取出来。也可以指定存放到特定的域中，默认是page域中。

<c:if>

```java
<c:if test="">执行语句</c:if>
<c:if test="" var ="flag" scope="">
将执行结果放在变量flag中，再将flag存放在指定域中。
```

test里面写的是EL表达式：如果test满足条件，则执行执行语句。

<c:foreach>

```Java
<c:forEach begin="1" end="10" var="i" step="2">
    ${i}
</c:forEach>
```



foreach循环，从begin开始到end，步数自己确定，items表示便利哪一个对象，这里必修写EL表达式，var 遍历的每一个元素都用该变量去接收。



### 页面转换单元：

被转换成Servlet类的页面集合称为转换单元 js;