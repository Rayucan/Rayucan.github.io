---
title: Nacos持久化配置切换Mysql时无法正常启动
date: 2021-02-19 22:25:09
tags: SpringCloud
categories: 问题解决
---

在Nacos单机模式部署Mysql过程中，配置完application.properties文件，发现`startup.cmd -m standalone`无法正常启动Nacos

<!-- more -->

网上查看很多解决方法，先是怀疑mysql 8.0版本过高不能适配Nacos，然后卸载重装了mysql 5.7版本，**但异常仍然存在**

再经过仔细查阅后发现一篇文章中提到：**bootstartp加载顺序优先于application**

![img](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210219223457.png)



经过测试，最终的解决方案：

~~**application.properties复制并且重命名为bootstrap.properties，在bootstrap.properties中进行配置**~~

------

> 更新

另外两种解决方案：

* 对于Mysql8.0+（由于mysql 8.0版本已经卸载，并未测试）

  去下载一个mysql-connector-java-8.0.x.jar

  http://mvnrepository.com/artifact/mysql/mysql-connector-java

  放在nacos\plugins\mysql目录下

* 在官方文档给出的配置代码基础上添加`serverTimezone=UTC`

  ```properties
  spring.datasource.platform=mysql
  
  db.num=1
  db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&serverTimezone=UTC
  db.user=root
  db.password=root
  ```

  