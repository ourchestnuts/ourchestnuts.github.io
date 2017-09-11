---
layout: post
title:  "jsp"
date:   2017-08-26
categories: jsp学习
---
####一）脚本Scriptlet   一般写在<body>中

1.<%......%>   定义局部变量、编写Java语句的     
2.<%! ......%>  定于全局变量、定义方法   
3.<%=.......%>  用来输出=后面的表达式的值，功能类似于out.print();   
>注意：out.print()和<%=… >不仅能输出变量，还可解析 ` ”&lt;br /&gt;” ` 等html代码。需要<%= …>中没有“;”

####二）指令

<%@.........%>
page指令   设置当前JSP文件的一些属性      
><%@ page language="java" import="java.util.*" contentType="text/html; charset=UTF-8"  pageEncoding="UTF-8"%>
        属性 | 说明
            -|-
language     |   指定JSP页面使用的脚本语言，默认是java，一般不用修改
import       |      与java中的import的用法一致，可以执行导包操作
pageEncoding |指定JSP文件本身的编码方式
contentType  |  指定服务器发送给客户端的的内容的编码方式，通常与pageEncoding保持一致

####三）注释

<!-- ....-->   HTML注释，用来注释HTML代码，可以通过浏览器查看到，因此是不安全的
<%-- ...--%>  JSP	注释 不可被浏览器看到
<%...%>内可用//单行注释   /*。。。*/多行注释             Java注释

HTML的注释能被浏览器所查看到，而JSP注释和JAVA注释不能被查看

####四）内置对象
没有定义和实例化（new）就可以直接使用的对象
内置对象类型简介1.pageContextjavax.servlet.jsp.PageContextJSP页面容器2.requestjavax.servlet.http.HttpServletRequest客户端向服务端发送的请求信息3.responsejavax.servlet.http.HttpServletResponse服务器端向客户端的响应信息4.sessionjavax.servlet.http.HttpSession客户端与服务器端的一次会话5.applicationjavax.servlet.ServletContext可存放全局变量，实现用户间数据的共享6.configjavax.servlet.ServletConfig服务器配置信息，可以取得初始化参数7.outjavax.servlet.jsp.JspWriter向客户端输出内容8.pagejava.lang.Object当前JSP页面本身，类似于Java类中的this关键字9.exceptionjava.lang.Throwable当一个页面在运行过程中发生了异常，就产生这个对象
#####1.out
out用于向客户端输出数据，最常用的是out.print();需要注意的是，out.println()或者out.print(“\n”)均不能实现在客户端的换行功能
换行必须借助<br/>标签换行
#####2.request
request对象主要用于存储“客户端发送给服务端的请求信息”，可以通过request对象来获取用户发送的相关数据
######常用方法：
方法简介public String getParameter(String name)获取客户端发送给服务端的参数值（由name指定的唯一参数值，如单选框、密码框的值）public String[] getParameterValues(String name)获取客户端发送给服务端的参数值（由name指定的多个参数值，如复选框的值）public void setCharacterEncoding(String env) throws java.io.UnsupportedEncodingException指定请求的编码，用于解决乱码问题public RequestDispatcher getRequestDispatcher(String path)返回RequestDispatcher对象，该对象的forward()方法用于转发请求public HttpSession getSession()返回和请求相关Sessionpublic ServletContext getServletContext()获取web应用的ServletContext对象
通过request.setCharacterEncoding("UTF-8")将POST方式的编码设置为UTF-8
通过request.getParameter()和request.getParameterValues()方法获取到了从表单传来的数据
客户端的数据除了从表单传递，也可以通过URL地址传递
格式：页面地址?参数名1=参数内容1&参数名2=参数内容2&…

######get post区别
<form action="show.jsp" method="post">
form属性method用来指定表单的提交方式，常用属性get和post
以post方式请求表单，请求后的地址为http://localhost:8080/JspProject/show.jsp
以get 方式请求表单，地址栏就会显示 http://localhost:8080/JspProject/show.jsp?uname=张三&upwd=123&hobby=足球&hobby=篮球
使用get方式请求表单实际就是通过URL地址提交的方式向服务器端发送数据
get和post方式在提交的数据大小上也有区别。
get方式会在地址栏上显示数据信息，而地址栏中能容纳的信息长度是有限制的（一般是4-5KB）； 
post方式不会在地址栏中显示数据信息，所以能提交更多的数据内容。因此，如果表单中有一些大文本、图文、文件、视频等数据，就必须使用post的方式提交。

######统一字符集编码
tomcat服务器，默认使用的编码方式是ISO-8859-1。
（1）如果是以get方式提交表单（或URL地址传递的方式），则有两种方式处理编码：
①分别把每一个变量的编码方式，从ISO-8859-1转为UTF-8
②修改tomcat配置，一次性的，将所有通过get方式传递的变量编码都设置为UTF-8（推荐）。具体修改如下：打开tomcat的conf目录，在server.xml的64行附近的<Connector>元素中，加入URIEncoding=”UTF-8”，
#####3.reponse
服务端可以通过response向客户端作出反应
方法简介public void addCookie(Cookie cookie)服务端向客户端增加Cookie对象public void sendRedirect(String location) throws IOException将客户端发来的请求，重新定位(跳转)到另一个URL上(习惯上称为“重定向”)public void setContentType(String type)设置服务器端响应的contentType类型
注意：cookie不是内置对象 不能直接使用，使用前必须创建
可以发现， 采用了request.getRequestDispatcher("success.jsp").forward(request, response)来跳转页面后：
1.就可以获取到客户端发送的表单数据；
2.页面内容确实跳转到了success.jsp中编写的内容，但地址栏却仍然停留在check.jsp，即采用请求转发方式，地址栏不会发生改变。
关于请求转发(request.getRequestDispatcher("xx").forward(request, response))和重定向response.sendRedirect("xx")的区别，经常会在面试中被提到，我们在此做一个总结，如下表：
请求转发(forward())重定向(redirect())请求服务器次数1次2次是否保留第一次请求时request范围中的属性保留不保留地址栏里的请求URL，是否改变不变改变为重定向之后的新目标URL。相当于在地址栏里重新输入URL后，再按回车键
#####4.session
会话   用户通过浏览器与服务器之间进行的一系列的交互过程，交互期间可以包含浏览器与服务器之间的多次请求、响应。
session内置对象是javax.servlet.ServletContext.HttpSession接口的实例化对象

方法简介public String getId()获取sessionIdpublic boolean isNew()判断是否是新的session（新用户）public void invalidate()使session失效public void setAttribute(String name, Object value)设置session对象名和对象值public Object getAttribute(String name)根据session对象名，获取session对象值public void setMaxInactiveInterval(int interval)设置session的有效非活动时间, 单位是秒public int getMaxInactiveInterval()获取session的有效非活动时间，单位是秒

方面|cookie|session
-|-|-
保存信息的位置		|客户端	|			服务器端
保存的内容			|字符串	|			对象
安全性 				|不安全	|			安全|
#####5.application
application对象是javax.servlet.ServletContext接口的实例化对象，代表了整个web项目，所以application对象的数据可以在整个web项目中共享
简介public String getContextPath()获取虚拟路径（默认是项目名称）public String getRealPath(String path)获取虚拟路径对应的绝对路径
换一个浏览器也可以显示
四种作用域的大小依次是pageContext<request<session<application