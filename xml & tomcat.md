# xml & tomcat

## xml

#### xml有什么用

1，可以用来保存数据

2，可以用来做配置文件

3，可以作为数据传输的载体

#### 定义xml

1，文档声明

<? xml version = "1.0" encoding = "utf-8"?>

<? xml version = "1.0" encoding = "utf-8" standalone = "no"?>  : 这是一个独立的文档

2，元素的定义
<>里面的是标签，也叫元素，所有标签都必须闭合，空标签：即是开始又是结束。标签名可可以自己定义。

3，标签命名规则：尽量简单，见名知意

## xml里的CDATA区：

非法字符：在xml里面仅有字符“<”和字符"&"是非法的，省略号、引号，大于号都是合法的，但是把它们替换为实体引用类是个好习惯

<  替换为”&lt";  & 替换为“&amp"

如果某个字符里面有过多的字符，并且里面包含了类似标签或者关键字的这种文字，不想让xml的解析器区解析，那么可以用CDATA来包装，不过这个CDATA一般很少看到，通常在服务器给客户返回数据的时候可能会见到，

## xml内容的解析：

#### xml的解析方式有2种：DOM和SAX

其实就是获取元素里面的字符数据或者属性数据。

DOM：document object model 把整个xml文件全部读入到内存当中，形成树状结构，称之为document对象，属性对应Attribute，所有的元素节点对应为Element对象，文本也可以称之为Text对象，以上所有的对象都可以称之为Node节点。如果xml特别大，那么会造成内存溢出。如果文档比较小的话，速度比较快，可以对文档进行增删的操作。

SAX: simple api for xml 基于事件驱动(读一行，解析一行) ,不会造成内存溢出，不能进行增删，只能进行查询。

##### 针对这两种解析方式的API：

jaxp	jdom	dom4j(运用比较广泛)

### DOM4J入门：

1，创建SaxReader对象

2，指定解析的xml

3，获取根元素

4，根据根元素获取子元素或者下面的子孙元素

code:

​		try{

​				//1,创建sax读取对象

​				SAXReader reader = new SAXReader();

​				//2,指定解析的xml源

​				Document document = reader.read(new File("src/xml/stus.xml"));

​				//3,得到根元素

​				Element rootElement = document.getRootElement();

​				//4,根据根元素的到根元素的子元素

​				rootElement.element("stu").element("name").getText();

​				

```java
public static void main(String[] args) throws DocumentException {
    System.out.println("Hello World!");
    //1,创建SAX读取对象
    SAXReader reader = new SAXReader();
    //2，指定解析的xml文件
    Document document = reader.read(new File("src/xml/stus.xml"));
    //3,得到根元素
    Element rootelement = document.getRootElement();
    System.out.println(rootelement.getName());
    //获取根元素下的子元素age
    rootelement.element("name");
    System.out.println(rootelement.element("stu").element("name").getText());
    List<Element> elementList = rootelement.elements();
    for (Element element:elementList) {
        String name = element.element("name").getText();
        String age = element.element("age").getText();
        String gender = element.element("gender").getText();
        System.out.println("name:" +name + " age: " + age + "gender" +  gender);
    }
}
```



3，得到指定文件的根元素

4.再根据根元素获取其下的各个元素。	

 

### DOM4J的Xpath的使用:

1,dom4j 里面支持Xpath的写法。xpath其实是xml的路径语言，支持我们在解析xml的时候能够快速的找到具体的某一个元素

2，首先添加jar包，：jaxen-1.1-beta-6.jar

3,在查找指定节点的时候，根据xpath语法规则来查找

4，后续的代码于以前的解析代码一样

### xml的约束：

xml里面的属性唯一存在，如两个人的id号一样，这在实际生活中是不可能存在的，规定元素

##### DTD

dtd的语法自成一派，早期出现的可读性比较差

##### SCHEMA

其实就是一个xml文件，使用xml的语法规则，xml解析器解析起来比较方便，是为了替代dtd，但是schema约束文件内容比较多，实际上并没有真正意义上的替代dtd



#### 三种引用dtd的方法：

​					文档类型     根标签名     网络的dtd	dtd名称	dtd路径

1 ，引用网络dtd文件：<!DOCTYPE STUS   PUBLIC " UNKNOWN/" "UNKNOWN.DTD">

​					文档类型		根标签名	本地dtd 	dtd的位置

2，引用本地的dtd文件： <!DOCTYPE stus 	SYSTEM	" stus.dtd">

3，直接在xml里面嵌入dtd的约束规则

<!DOCTYPE stus[


​				<!ELEMENT stus  (stu)>	（stus下面只有一个元素stu)

​				<!ELEMENT   stu	(name,age)>

​				<!ELEMENT 	name	(#PCDATA)>

​				<!ELEMENT 	age	(#PCDATA)>

]

（#PCDATA)被解析的数据

4,<!ATTLIST >描述元素的属性

5，元素的个数：+ * ？

6，属性的类型定义：

​	CDTAT:属性的值是普通文字

​	ID: 书写的值必须唯一

​	<ATTLIST stu id ID  #IMPLED>

​	<ATTLIST stu id CDATA  #Requred>

​	<ATTLIST  stu  id CDATA  REQURED>必须有

#### schema约束：

1，首先，teachers.xsd文件也是一个xml文件，但是它的约束文件已经写好，

eg：

```java
<?xml version="1.0" encoding="UTF-8" ?>
<schema xmlns="http://www.w3.org/2001/XMLSchema">
    <element name="teachers">
        <complexType>
            <sequence>
                <element name="teacher">
                    <complexType>
                        <sequence>
                            <element name="name" type="string"></element>
                            <element name="age" type="int"></element>
                            <element name="gender" type="string"></element>
                        </sequence>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>

</schema>
```

引用schema的用法和dtd的用法不一样

##### 名称空间的作用：

一个xml文件只能引用一个dtd的约束条件，不能指定多个dtd约束，但是xml的约束条件是定义在schema里面的话，并且是多个s'che'ma，那么是可以引用多个的，简单地说，一个xml文件可以引用多个schema文件，但是只能引用一个dtd文件。

###### 也就是说名称空间有点类似于Java里的包

约束的引用写在根标签里面。

#### .xsd文件的定义

.xsd文件也是一个xml文件，根标签是<schema>根标签里面有三个属性：

#### xmlns、targetNamespace、elementFormDefault必须写

```java
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
         targetNamespace="http://www.itheima.com/teacher"
         elementFormDefault="qualified">
    <element name="teachers">
        <complexType>
            <sequence>
                <element name="teacher">
                    <complexType>
                        <sequence>
                            <element name="name" type="string"></element>
                            <element name="age" type="int"></element>
                            <element name="gender" type="string"></element>
                        </sequence>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>

</schema>
```



#### schema的引用

<teachers 

​	xmlns  =  "http://www.itheima.com/teacher" (这是.xsd文件里面定义的目标地址)

​	xmlns:xsi = " http://www.w3.org/2001/XMLSchema-instance"（这是w3c固定不变的的）

​	xsi:schemaLocation="http://www.itheima.com/teacher  teachers.xsd"（这个是目标地址和.xsd文件存放的位置



# tomcat

### 程序的架构：

##### c/s架构：qq，微信， LOL

优点：有一部分代码写在客户端，用户体验比较好

缺点：比较吃硬盘服务器更新，客户端也要随着更新，占用资源多

##### B/S架构：网页游戏、webQQ、。。。

优点：客户端只要有浏览器就好，占用资源小，不用更新

缺点：用户体验差，

### web服务器：

处理客户端的请求，返回资源 | 信息：客户端在浏览器地址栏上输入地址，然后web服务器软件响应

##### 其实服务器也是电脑，只不过配置比一般电脑高，

web应用：tomcat 	apache

​		   weblogic	bea

​		   websphere    ibm

1,直接解压，然后找到bin/startup.bat

#### tomcat目录

bin：包含了一些jar包，bat文件，

conf:包含了tomcat的一些配置文件，   server.xml    web.xml 。。。。。

lib：tomcat运行所需的jar文件

logs：运行的日志文件

temp：临时文件

webapps：网页的应用程序，发布在tomcat服务器上的项目，就存放在这个文件夹里。

work:  tomcat把jsp生成的servlet文件存放在该文件夹下

#### 把项目放到tomcat的服务器上：（3种方法）

1，拷贝这个文件到webapps/ROOT底下，在浏览器里访问：

​		http://localhost:8080/stu.xml

在webapps下面创建一个文件夹xml，然后拷贝文件到这个文件夹里，在访问

​		http://localhost:8080/xml/ stu.xml

2,在阿帕奇官网查看文档。配置虚拟路径

<在server.xml文件中找到<Host>标签，添加属性<Context docBase="文件路径" path="虚拟路径"></Context>,在浏览器地址栏上输入：http://localhost:8080/"虚拟路径"/文件名。

3，配置虚拟路径

在tomcat/conf/catalina/localhost/下新建一个.xml文件，名字自定义，person.xml

2,在这个文件里面写入一下内容：

​	<?xml  version="1.0" encoding="utf-8"?>

​	<Context docBase = "文件路径"></Context>

在浏览器上访问：http://localhost:8080/person/xml文件