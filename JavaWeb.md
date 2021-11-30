## Tomcat

官网下载https://tomcat.apache.org/download-90.cgi

![image-20201121200154790](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20201121200154790.png)

> bin

​		startup.bat用来开启tomcat

​		shutdown.bat用来关闭tomcat

​		默认的访问网址localhost:8080

> config

​	server.xml主要用到的配置文件

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
<!--设置默认端口为8080使用协议超时喝重定向端口-->


<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
<!--设置默认域名localhost,网址存放目录为webapps-->
    如果要修改属性name就要修改
    C:\Windows\System32\drivers\etc\hosts文件(本机路由文件)
```

## Maven

官网下载https://maven.apache.org/download.cgi

然后修改配以依赖源

在setting.xml文件中镜像修改为下面

```xml
<mirrors>
  <mirror>
    <id>alimaven</id>
    <mirrorOf>central</mirrorOf>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
  </mirror>


  <!-- 中央仓库1 -->
  <mirror>
    <id>repo1</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://repo1.maven.org/maven2/</url>
  </mirror>


  <!-- 中央仓库2 -->
  <mirror>
    <id>repo2</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://repo2.maven.org/maven2/</url>
  </mirror>
</mirrors>
```

创建项目

Maven的GAV(groupld,Artifact,Version){com.kuang,javaweb-01-web,默认}

![image-20201121205250279](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20201121205250279.png)

在maven中设置启动项

![image-20201121205452192](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20201121205452192.png)

## 在build中配置resource来防止导出失败

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
            </excludes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

添加上面这串代码在配置文件中



## servlet

新建一个servlet程序

1.新建项目可以建立一个空项目或者选择一个模板

![image-20201123103615028](C:\Users\lll\AppData\Roaming\Typora\typora-user-images\image-20201123103615028.png)

2.在main下面添加Java和resource资源文件夹

3.在Java里面创建一个继承HttpServlet的子类重写doGet和doPost方法

4.在web.xml将资源修改为最新的如:

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
```

添加映射

```xml
<!--    注册-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.YH.servlet.HelloServlet</servlet-class>
    </servlet>
<!--    请求-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/waibi</url-pattern>
    </servlet-mapping>
```

5.配置tomcat



### 下载文件

```java
		//获取文件地址
		String path = "D:\\diary\\JUC.md";
		//获取文件的流对象
        FileInputStream inputStream = new FileInputStream("D:\\diary\\JUC.md");
		//获取文件名
        String filename = path.substring(path.lastIndexOf("\\")+1);
		//设置响应内容使其识别该文件（如果不设置filename传输格式和文件名为默认路径名称）
        resp.addHeader("Content-Disposition", "attachment;filename="+filename);
        System.out.println(filename);
		//开始用流进行传输
        int len;
        byte[] stream = new byte[1024];
        ServletOutputStream outputStream = resp.getOutputStream();
        while ((len=inputStream.read(stream))!=-1)
        {
            outputStream.write(stream,0,len);
        }
        outputStream.close();
        inputStream.close();
```

### 获取前端送来的参数



```java
public class redirect extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
 		//如果要读取类似复选框这种多个数据可以用req.getParameterValues();
        System.out.println(req.getServletPath());
        if(username!=null && password != null && username.equals("123456") && password.equals("123456"))
        {
            resp.sendRedirect("/servlet_02_war/result.jsp");
        }
        else
        {
            resp.sendRedirect("/servlet_02_war/login.jsp");
        }
    }
}
```

get和post请求都可以用





## JSP简单的语法介绍

```jsp
<%= new Date(); %>  <%--将代码结果直接在浏览器中输出--%>
<%! private int k = 10;%> <%进行预定义%>
<%@ include file = "..."%>可以导入一个页面
	<jsp:include page="">这种导入方式不会吧所有的源代码导入
<%  out.print("aaa");  %> <%--执行Java语言块--%>

<%int a = 1;%>
<%int a = 2;%>
<%--这样不行会报错因为会把源码直接copy到运行文件中--%>

<% 
	for(int i = 0;i<=10;i++){
    %>
<h1>aaa</h1>
<%
	}
%>
<%--这样可以循环生成标签--%>

${}中的值不会出现null
<%=%>取出的值会出现null
```

## 九大对象

1.PageContext 存东西 保存的东西只在一个页面中有效

2.Request 存东西 一次请求中有效请求转发也有效

3.Response

4.Session 存东西 在一次会话中有效 从打开浏览器到关闭浏览器

5.application[servletContext] 存东西 在服务器中有效,从打开服务器到关闭服务器(AB浏览器)

6.config[servletConfig]

7.out

8.Page

9.exception

## Cookie

示例代码
		

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//浏览器提交请求会自动把Cookie传递到我们的服务器
		//获取Cookie	
    Cookie[] cooks = request.getCookies();
	for (Cookie cookie : cooks) {
		System.out.println(cookie.getName() +" "+URLDecoder.decode(cookie.getValue(),"utf-8"));
	}
	
	//Cookie是一个键值对
	Cookie c= new Cookie("name","aaa");
	//设置保存时间 （默认的保留时间是一个会话）
	c.setMaxAge(3600);//时间单位为int，表示秒 ，时间单位为long，表示毫秒
		//--只是创建cookie，还需要通过response的addCookie添加到客户端
	response.addCookie(c);
	
	//Cookie不支持直接保存中文，如果要存中文，创建cookie时需要对它进行编码，获取cookie时需要对它解码
	//对字符串进行编码   URLEncoder.encoder(s);
	//对字符串进行解码 URLDecoder.decoder(s);
	
}
```



使用方法

- Cookie cookie=new Cookie（“key”，“value”）；

- set/getName() //获取key

- set/getValue()

- set/getMaxAge() 失效时间 单位秒

  

不能保存中文！！！

```java
Cookie c = new Cookie(“name”, URLEncoder.encode(“中文”,“utf-8”));
```

只能用上面这种方式来保存





## session

执行过程：

当浏览器第一次访问时会生成以可session并且把session的ID添加到cookie中这样浏览器就会有一个关于这个id的cookie了



```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//每次创建一个新的setlvet  需要重启一下服务器
		
		//session 是服务器端的一块存储空间  是一个map结构 
		//作用是 保存用户登录信息 
		
		//获取session对象 request.getSession()
		HttpSession session= request.getSession();
		
		//设置值 
		session.setAttribute("name", "gok");
		session.setAttribute("UserInf", "gok1");
		//和获取值 
		System.out.println(session.getAttribute("name"));
		
		//移除属性
 
		session.removeAttribute("name");
		
		//销毁session 这个对象就没了   
  //【默认session生命周期是30分钟 这个时间是可以改的 在web.xml 】
		// <session-config>
		// <session-timeout>100</session-timeout>
		// </session-config>
          //销毁session 
	 //	session.invalidate(); 
		response.getWriter().println("中文1111但是撒多撒");

		
	}

```



## 过滤器

过滤器会在tomcat启动时一起启动销毁时一起销毁

过滤器需要绑定域名



建立过滤类并实现doFilter中必须假如

> filterChain.doFilter(servletRequest,servletResponse);

否则无法继续运行



例如：

通过添加过滤器来过滤全网的编码

在web.xml中添加如下代码

```xml
<filter>
    <filter-name>EncodeSet</filter-name>
    <filter-class>filter.EncodeFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>EncodeSet</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



实现Filter类

```java
package filter;


import javax.servlet.*;
import java.io.IOException;

public class EncodeFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("过滤中....");
        servletRequest.setCharacterEncoding("UTF-8");
        servletResponse.setContentType("text/html;charset=utf-8");
        filterChain.doFilter(servletRequest,servletResponse);
        System.out.println("过滤完毕...");
    }

    public void destroy() {

    }
}
```

## Listener和Filter

```xml
<filter>
    <filter-name>waibi</filter-name>
    <filter-class>FilterOne</filter-class>
</filter>
<filter-mapping>
    <filter-name>waibi</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<listener>
    <listener-class>listener</listener-class>
</listener>
```



首先需要注册Filter类和Listener

Listener监听的是cookie或session等（可以实现用户统计，如当用户登录到页面并创建session时调用一次监听器）

Filter过滤的是根据URL来进行过滤（可以实现统一修改字符集，服务器开启时开启，服务器关闭时关闭）



## 其他



##### 设置页面自动刷新

response.setIntHeader("Refresh", 5);



##### Cookie和Session：

1. cookie和session都是用来存放数据的Cookie的存放形式单一，键值对（key，value）形式，而session没有固定的可以存放数据、文件、数据库文件等等
2. cookie在浏览器上，session放在服务器上。
3. 账户密码不要放在cookie上不安全
4. cookie和session都有有效时间，每次使用都会加满有效时间



##### 关于优先级

mapping中的请求路径设置为/*可以作为作为error页面

如果指定了固有路径则优先级最高默认最低



##### servlet的应用

https://www.cnblogs.com/ful1021/p/4804321.html



重定向和转发的区别：

 相同点：都是跳转页面

 不同点：重定向会改变地址，转发不会

​		重定向/代表跟目录而转发的/代表当前tomcat目录

​		例如在https://www.baihao.com/YH/666中进行

​			转发/代表https://www.baihao.com/YH/

​			而重定向道标https://www.baihao.com

```java
req.getContextPath()
//可以获取当前servlet项目


//转发
req.getRequestDispatcher("/路径").forward(req,resp);

//重定向
resp.sendRedirect("/servlet_02_war/result.jsp");
```

##### 从properties文件中读取配置

```java
package com.YH.dao;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class BaseDao {
    private static String Driver;
    private static String Url;
    private static String Username;
    private static String Password;

    static {
        //将文件读取成流
        InputStream stream = BaseDao.class.getClassLoader().getResourceAsStream("db.properties");
        //新建一个配置文件类
        Properties properties = new Properties();
        try {
            //加载流
            properties.load(stream);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //从流中读取数据
        Driver = properties.getProperty("driver");
        Url = properties.getProperty("url");
        Username = properties.getProperty("user");
        Password = properties.getProperty("password");
    }
    //获取连接
    public static Connection getConnection() throws ClassNotFoundException, SQLException {
        //加载驱动
        Class<?> aClass = Class.forName(Driver);
        //获取连接
        Connection connection = DriverManager.getConnection(Url);
        return connection;

    }
}

```

这个别的时间再练把

