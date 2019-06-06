# servletRequest  和  httpServletResponce:

### servlet的配置方式：

1，全路径匹配：

以 /  开始  /a、/aa、/fskfj/fdjks........

​	localhost :  8080/项目名/a

2,路径匹配：

​	以/开始，但是以*结束    

​	其实是一个通配符 匹配任何文字 

3，以扩展名匹配

写法：  * . 扩展名

## servletContext

servlet上下文

每一个web工程都只有一个servletcontext对象，说白了也就是不管在哪个servlet里面获取的这个类都是同一个对象。

##### 如何得到对象？

1，servletContext context  = getServletContext();

##### 有什么用？

1，可以获取全局参数	

首先设置全局参数：

<context-param>

​	<param-name>address</param-name>

​	<param-value>西安</param-value>

</context-param>

然后在servlet里面用servletContext获取到的对象获取参数：

ServletContext context = getServletContext();

String address =   context.getInitParameter("address");

#### ServletContext普通方式获取工程文件：

用request获取头信息：

```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Enumeration<String> headernames = request.getHeaderNames();
    while(headernames.hasMoreElements()){
        String name = (String)headernames.nextElement();
        String value = request.getHeader(name);
        System.out.println(name + ":=" + value);
```

用request获取提交的信息：

```
  }
    String name = request.getParameter("name");
    String address = request.getParameter("address");
    System.out.println("name:"+ name);
    System.out.println(("address:"+address));
}
```

# Cookie:

> 饼干：其实是一份小数据，在服务器给客户端的一份数据并且存储在客户端上的一份小数据
>
> 应用场景：自动登录、浏览记录、 购物车。

###### 为什么要又cookie这个东西？

1，http的请求是无状态的。客户端和服务器在第二次连接的时候，服务器不知道该客户端之前是否连接过。为了更好的用户体验，出现了cookie。

2，

  



## Request中文乱码问题；

1，先让文字回到ISO-8859-1对应的字节数组，然后再按照utf-8拼接字符串	

​	username  = new String(username.getBytes("ISO-8859-1"),	"UTF-8");

​	system.out.println(username);此方法对于get方法和post方法都可以用

2，但是request.setCharacterEncoding("utf-8");只适用于post方法；

## Responce中文乱码问题：

相应的内容有中文，极易出现中文乱码，

#### 以字符流输出：

-----》response.getWriter().write("我爱中国");

1,指定输出到客户端的时候，这些文字是哦给你utf-8编码：response.setCharacterEncoding("utf-8");

2,直接规定浏览器在看着分数据的时候，使用什么编码来看：response.setHeader("Content-Type","text/html";charset("utf-8"));-

#### 以字节流输出：

-----》response.getOutputStream().write("我爱你中国".getBytes());

-----》response.getOUtputStream().write("我爱你中国".getBytes("GBK"));

如果想要服务器出去的中文在客户端能够正常显示，只要确保一点，出去时候的编码和客户端看这份数据用的编码是一样的就好了。默认情况下这个方法用的是utf-8的码。