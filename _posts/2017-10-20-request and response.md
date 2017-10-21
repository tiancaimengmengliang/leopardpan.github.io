---
layout: post
title: "request and response"
date: 2017-10-20
description: "请求对象request和响应对象response"
tag: JavaEE
---


### 请求对象request和响应对象response

----------


### 内容概要


>  - 请求和响应的路径问题
 - 请求对象HttpServletRequest概述
 - HttpServletRequest头信息的获取
 - HttpServletRequest参数接受
 - 请求参数接受的中文乱码问题
 - Request请求转发和域
 - HttpServletResponse

----------

### 路径访问Servlet



> - 相对路径访问Servlet
 - 当前目录用./表示
 - 上一级目录用../表示
> - 绝对路径访问Servlet
 - 带有协议的http://localhost:8080/项目名/xxx
        - 缺点：路径是固定的
 -  /项目名/xxx
        - “/”代表前面的ip和端口
        - “/”在客户端浏览器是代表协议、ip和端口（可以体现在html等的编写上，从浏览器上可获取的资源）
        - “/”在服务器中（服务端页面跳转）代表的是协议、ip、端口和项目名


----------


### 请求对象HttpServletRequest概述


>  - Web服务器收到客户的Http请求，会针对每一次请求，分别创建一个代表请求的Request对象和代表响应的Response对象。
>  - 想获取客户提交过来的数据，需要Request，想要返回给客户，需要response


----------

### HttpServletRequest头信息的获取



>- HttpServletRequest在JavaWeb中是非常重要的一个类，它是Servlet的Service()的方法参数之一
> - request的功能可以分为以下几种
    - 封装了请求头数据
    - 封装了请求正文数据，如果是Get请求，那么没有正文
    - request是一个域对象，可以把它当成Map来添加获取数据
    
> - Request获取请求头数据
    - 公用接口 HttpServletRequest
    扩充ServletRequest扩充ServletRequest 
    接口 以为HTTP servlet提供信息. 
    servlet 容器创建了一个HttpServletRequest对象并将之作为一个参数传送给 servlet's service 方法 (如doGet, doPost等).
    
> - request对象可以用来获取请求头数据，这些请求数据都是Tomcat封装的
> - 有关HttpServletRequest的方法
    - String getHeader(String name),返回指定的作为字符串请求消息头的值
    - Enumeration getHeaderNames(),返回所有的本请求消息包含的头名字的集合。 
    - int getIntHeader(String name),返回一个指定的请求消息头的整数值。

----------


    package com.rl.servlet;

    import java.io.IOException;
    import java.io.PrintWriter;
    import java.util.Enumeration;

    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    public class hellloservlet extends HttpServlet {

	    public void doGet(HttpServletRequest request, HttpServletResponse response)
	        throws ServletException, IOException {
		    //根据头信息的key来获得值
		    String headValue = request.getHeader("Host");
	    	System.out.println("Host:" + headValue);
		
		    //活得头信息的name
		    Enumeration headNames = request.getHeaderNames();
		    while(headNames.hasMoreElements()){
			    String name = (String) headNames.nextElement();
			    System.out.println(name + ":" + request.getHeader(name));
	    	}
	    }
    }

> - request 获取请求相关的其他方法，有些方法是为了我们更加便捷的方法而设计，有些是请求Url相关的方法
    - int getContentLength()，返回请求体的用字节表示的长度，如果长度未知，就返回-1，eg：doget()没有正文返回-1。继承自接口ServletRequest的方法。
    - **String getContentType()**获取请求类型，如果请求是Get，那么这个方法返回null,如果是Post类型，那么默认为application/x-www-from-urlencoded(默认理解为字符串类型)，来自接口ServletRequest的方法
    - String getMethod()返回用以作出请求的HTTP方法的名称，例如 GET, POST, PUT等。
    - Local getLocal()，返回当前客户端浏览器所支持Local。java.util.Local表示国家和言语，国际化中很有用。
 - String getCharacterEncoding()，返回用在请求信息的body编码的字符的名称。如果没有设置，那么返回null，表示使用ISO-8859-1编码。
 - void setCharacterEmcoding(String code)，设置请求编码，只对正文有效，只对Post中的参数中、有效。（J2EE6中没有这个方法）
 - String getContextPath()，返回上下文路径
 - String getQueryString()，返回请求url中的参数
 - String getRequestURI()，返回请求url中的路径 eg：/hello/oneservlet
 - StringBuffer gatRequestURL()，返回请求url中的路径 eg：http://localhost:8080//hello/oneservlet
 - String getRemoteAddr()，返回当前客户端的IP地址
 - 更多请参考API
 
----------

 >  - 下面是一个实例，http://localhost:8080/hello/oneServlet?name=zhangsan
    - request.getContextPath()：/hello
    - request.getQueryString()：name=zhangsan
    - request.getRequestURI：/hello/oneServlet
    - request.getRequestURL()：http://localhost/hello/oneServlet
    - request.getServletPath()：/oneServlet
    - request.getScheme()：http
    - request.getServletName：localhost
    - request.getServerPort：8080

----------
    
### HttpServletRequest参数接受


>  - HttpServletReauest获取客户端的参数请求，一般有以下几种
    - String getParameter(String name)，返回一个请求参数的字符串值。若该参数不存在，则返回一个空值。
    - String[] getParameterValues(String name)，返回一个包含所有的给定请求参数的值的字符串对象的向量。一个name可能有多个值，例如：复选框
    - Enumeration getParameterNames()，返回一个包含了请求中的参数名字的字符串对象的枚举变量。
    - Map getParameterMap()：获取参数对应的Map，key为参数，value为参数值
 - 传递参数的方式：一般是Get和Post
    - Get
        - 地址栏中给出参数 http://xxxx/xxxx
        - 超链接中给出 <a herf="http://xxx/xxx"
        - 表单中给出的参数 <form method="GET" action="Paramservlet">...</form>
        - Ajax暂不介绍
    - Post
        -  表单中给出的参数 <form method="POST" action="Paramservlet">...</form>
        - Ajax暂不介绍


----------

### 请求参数接受的中文乱码问题


>  - Request接收参数时，有get和post两种方式，但处理中文的编码却不一样，做项目时一般用UTF-8，可以设置全局工作模式，Windows下workspace选择UTF-8
 - 当使用Post方式时，请求消息中，Post编码有问题，正文使用ISO-8859-1，页面使用UTF-8编码，所以会有问题
 
 
----------
 
    String name = request.getParameter("name");
    name = new String(name.getBytes("ISO-8859-1"),"UTF-8");
    System.out.println(name);
> 因为在获取参数时已经被错误的编码了，本来使用的是UTF-8编码，但是请求正文还是用ISO-8859-1编码。可以先获取ISO-8859-1的字节数，在转换为UTF-8编码

> - 还可以使用request的setCharacterEncoding()来设置编码，指定正文的编码方式

----------

    request.setCharacterEncoding("UTF-8");
    String name = request.getParameter("name");
    

> - 当使用Get方式处理中文乱码时，处理乱码的方法一

----------

    String name = request.getParameter("name");
    name = new String(name.getBytes("ISO-8859-1"),"UTF-8");
    System.out.println(name);

> - 当使用Get方法请求时，正文内容不在请求正文中，而是在url中，所以不能设置request的setCharacterEncoding()来设置Get参数的编码。
    处理Get参数编码可以有两种编辑方式：第一种是设置<Connector>元素的URLEncoding属性值为UTf-8。即con\server.xml中的<Connector>元素的URLEncoding属性：
    
    ----------
    
        <Connector port='8080' protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" URIEncoding="UTF-8"
        />
> 一旦设置了这个属性，那么对于GET参数就直接是UTF-8编码的了，Connector元素修改的是Tomcat的全局属性。

> - 第三种设置Get方法的中文乱码的方法是，javascripts对超链接做url编码，即对Get请求中的参数使用javascripts做url编码，编码后的参数不再是中文，这样IE6也不会丢失字节了。

----------

### Request请求转发和域（服务器端跳转）



    public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		
		/**
		* request请求转发，注意路径，不用带项目名，直接转发
		* 服务器短的跳转是转发，地址栏不变，传递的是同一个request和response
		*/
		request.getRequestDispatcher("/xx.html").forward(request, response);
	}

> - 可以设置参数获取参数request.setAttribute(String name,Object o)、request.getAttribute(String name)返回值Object
- request和response在一个生命周期内，参数等都在这个请求范围内生存。


----------

### HttpServletResponse详解



>- 设置发送状态码 void setStatus(int sc)，void setSatus(int sc,String sm)，例如：setSatus(404,"页面找不到")
 - 设置响应头信息 void addHeader(String name,String value)，例如，response.   [addHeader("reFresh","5:urlxxx")，3s之后跳转到url，页面跳转不属于服务器跳转
 - 设置响应PrintWriter getWriter()向客户机发送消息，例如：response.getWriter().print("success");
 - 中文乱码处理问题
    response.setContentType("text.html,charset=utf-8");
    response.setCharacterEncoding("utf-8");
 - 重定向（客户端跳转，地址栏变化）
 response.sendRedirect("/项目/xxxx")
 
 **forward是服务器端的跳转，跳转地址栏不变，Redirect是客户端的跳转，跳转地址栏变化**
 

        

    

        

---


