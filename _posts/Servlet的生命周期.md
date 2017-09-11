---
layout: post
title:  "Servlet的生命周期"
date:   2017-09-26 
categories: jekyll update
---


Servlet是运行在服务器端的一段程序，生命周期受到Servlet容器事物影响

加载和卸载由servlet容器完成，

######1.初始化

当一个Servlet被加载完毕并实例化以后，Servlet容器将调用init()方法初始化这个对象，执行一些初始化的工作，如读取资源配置信息等。如果初始化阶段发生错误，此Servlet实例将被容器直接卸载。

######2.服务

初始化完成以后，Servlet就会去调用service()的具体实现方法doGet()或doPost()，来处理请求；并通过ServletRequest类型的参数接收客户端的请求，以及通过ServletResponse类型的参数处理响应信息。

######3.销毁

Servlet实例服务完毕以后，就可以通过destroy()方法来指明哪些资源可以被系统回收（注意destroy()方法只是“指明”需要被回收的方法，并不会直接进行回收）。

Servlet API由两个软件包组成：一个是对应HTTP的软件包，另一个是不对应HTTP的通用的软件包。
两个软件包同时存在使得Servlet API能够适应任何请求-响应协议。
ServletConfig接口
ServletConfig对象可以在Servlet初始化时，向Servlet传递信息。













