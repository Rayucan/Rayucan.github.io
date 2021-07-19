---
title: SpringBoot-Maven多模块项目打包时报错找不到主类
date: 2021-04-17 16:26:06
tags: SpringBoot
categories: 问题解决
---

在springboot项目进行maven install操作时，通用模块common报错`repackage failed: Unable to find main class`

<!-- more -->

原因：common模块中继承了父模块插件spring-boot-maven-plugin，打包时它会去寻找common模块的主启动类，**然而common模块本身没有主启动类**，所以报错

解决方法：目前项目的子模块较少，我直接把父模块pom中的spring-boot-maven-plugin注释掉，转移到common之外的其他子模块中，问题解决，以后若有更好的方法再议。