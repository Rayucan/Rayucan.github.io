---
title: Servlet
date: 2020-10-02 00:00:00
tags: Web
categories: 笔记
description: 用Java编写的服务器端程序。
---

## Servlet简介

1、Servlet 是 JavaSE 规范之一，规范就是接口
2、Servlet 是 JavaWeb 三大组件之一，三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器
3、Servlet 是运行在服务器上的一个 Java 小程序，它可以接收客户端发送过来的请求，并响应数据给客户端



## URL地址如何访问Servlet程序

![0l15UU.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150547.jpeg)



## Servlet程序生命周期

1、执行 Servlet 构造器方法
2、执行 int 初始化方法
第一、二步是在第一次访问时创建 Servlet 程序会调用

3、执行 service 方法
第三步，每次访问都会调用

4、执行 destroy 销毁方法
第四步，在 web 工程停止的时候调用



## Servlet继承体系

![0lGpjA.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150547.jpeg)



## 通过继承HttpServlet实现Servlet程序

一般在实际项目开发中，都是使用继承 HtpServlet 类的方式去实现 Servlet 程序

1、编写一个继承 HttpServlet 的类
2、根据业务需要重写 doGet 或 doPst 方法
3、 web.xml 配置 Servlet 程序访问地址



```java
public class HelloServlet extends HttpServlet {
    /*
    * doGet（）在 get 请求的时候调用
    * @parm req
    * @parm resp
    * @throws ServletException
    * @throws IOException
    */
    @Overide
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    					IOException {
        System.out.println("HeloServlet 的 doGet 方法");
    }
    /*
    * doPst（）在 post 请求的时候调用
    * @parm req
    * @parm resp
    * @throws ServletException
    * @throws IOException
    */
    @Overide
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    					IOException {
        System.out.println("HeloServlet 的 doPst 方法");
    }
} 
```



## HttpServletRequest类

### HtpServletRequest 类有什么作用
每次请求进入 Tomcat 服务器，Tomcat 服务器便把请求的 HTTP 协议信息解析封装到 Request 对象中，然后传递到 service() 方法（doGet 和 doPost），我们可以通过 HttpServletRquest 对象，获取所有请求的信息 

### HtpServletRequest 类的常用方法

i. getRequestURI() 获取请求的资源路径
ii. getRequestURL() 获取请求的统一资源定位符（绝对路径）
iii. getRemoteHost() 获取客户端的 ip 地址
iv. getHeader() 获取请求头
v. getParmetr() 获取请求的参数
vi. getParmetrValues() 获取请求的参数（多个值的时候使用）
vii. getMethod() 获取请求的方式 GET 或 POST
viii. setAtribute(key, value); 设置域数据
ix. getAtribute(key); 获取域数据
x. getRequestDispatcher() 获取请求转发对象



## HttpServletResponse类

### HttpServletResponse 类的作用

HttpServletRsponse 类和 HttpServletRquest 类一样。每次请求Tomcat 服务器都会创建一个 Response 对象传
递给 Servlet 程序。HttpServletRquest 表示请求过来的信息，HttpServletRsponse 表示所有响应的信息，
我们如果需要设置返回给客户端的信息，都可以通过 HttpServletRsponse 对象来进行设置

### 两个输出流

字节流 getOutpStream();	 //常用于下载（传递二进制数据）
字符流 getWrite(); 				//常用于回传字符串（常用）

两个流同时只能使用一个
即：使用了字节流，就不能再使用字符流，否则报错



## 请求转发

![0l8FW4.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150945.jpeg)



## 请求重定向

![0l8WkT.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150954.jpeg)







