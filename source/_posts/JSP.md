---
title: JSP
date: 2020-10-03 00:00:00
tags: Web
categories: 笔记
description: 由Sun公司主导创建的动态网页技术标准。
---

## JSP简介

jsp 即 java server pages，Java的服务器页面
jsp 的主要作用：代替 Servlet 程序回传 html 页面的数据。Servlet 程序回传 html 页面数据是一件非常繁锁的事情，开发成本和维护成本都极高



## JSP的本质

jsp 页面本质上是一个 Servlet 程序。
当我们第一次访问 jsp 页面的时候，Tomcat 服务器把 jsp 页面翻译为一个 java 源文件，并且编译成.class 字节码程序



## JSP语法

### jsp 头部的 page 指令

jsp 的 page 指令可以修改 jsp 页面中一些重要的属性，或者行为

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```



i. language 属性 表示 jsp 翻译后是什么语言文件。暂时只支持 java
ii. contentType 属性 表示 jsp 返回的数据类型是什么。也是源码中 response.ContentType()参数值
iii. pageEncoding 属性 表示当前 jsp 页面文件本身的字符集。
iv. import 属性 跟 java 源代码中一样。用于导包，导类。

========两个属性是给 out 输出流使用==========
v. autoFlush 属性 设置当 out 输出流缓冲区满了之后，是否自动刷新冲级区。默认值是 true
vi. buffer 属性 设置 out 缓冲区的大小。默认是 8kb

========两个属性是给 out 输出流使用==========
vii. errorPage 属性 设置当 jsp 页面运行时出错，自动跳转去的错误页面路径

viii. isErrorPage 属性 设置当前 jsp 页面是否是错误信息页面。默认是 false。如果是 true 可以
获取异常信息



ix. seesion 属性 设置访问当前 jsp 页面，是否会创建 HttpSesion 对象。默认是 true。

x. extends 属性 设置 jsp 翻译出来的 java 类默认继承谁。



### jsp 中的常用脚本

#### 声明脚本(极少使用)

声明脚本的格式是：

```jsp
 <%! 声明 java 代码 %>
```

作用：可以给 jsp 翻译出来的 java 类定义属性和方法甚至是静态代码块，内部类等。
练习：
1、声明类属性
2、声明 static 静态代码块
3、声明类方法
4、声明内部类



#### 表达式脚本（常用）

表达式脚本的格式是：

```jsp
<%=表达式%>
```

表达式脚本的作用是向 jsp 页面上输出数据
表达式脚本的特点：
1、所有的表达式脚本都会被翻译到jspService() 方法中
2、表达式脚本都会被翻译成为 out.print()输出到页面上
3、由于表达式脚本翻译的内容都在jspService() 方法中,所以_jspService()方法中的对象都可以直接使用
4、表达式脚本中的表达式**不能**以分号结束



#### 代码脚本

代码脚本的格式是：

```jsp
<%
java 语句
%>
```


代码脚本的作用是：可以在 jsp 页面中，编写我们自己需要的功能（写的是 java 语句）。
代码脚本的特点是：
1、代码脚本翻译之后都在jspService 方法中
2、代码脚本由于翻译到 jspService()方法中，所以在jspService()方法中的现有对象都可以直接使用。
3、还可以由多个代码脚本块组合完成一个完整的 java 语句。
4、代码脚本还可以和表达式脚本一起组合使用，在 jsp 页面上输出数据