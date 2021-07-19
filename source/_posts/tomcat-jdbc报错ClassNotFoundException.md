---
title: tomcat-jdbc报错ClassNotFoundException
date: 2021-06-12 14:53:23
tags: Web
categories: 问题解决
---

使用Servlet做登录验证功能时，启动Tomcat后，DAO层jdbc连接报错`java.lang.ClassNotFoundException: com.mysql.cj.jdbc.Driver`

<!-- more -->

事先我确实忘记导入`mysql-connector-java-x.x.x.jar`依赖了，但是导包之后重启tomcat，仍然报此错误

查找资料后发现原因是**tomcat不会查找web项目下的依赖，而是查找自身lib目录的依赖**

所以**需要把jar包拷贝到tomcat/lib目录下**

