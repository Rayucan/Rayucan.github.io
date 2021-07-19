---
title: Maven
date: 2020-10-05 00:00:00
tags: Web
categories: 笔记
description: 软件项目管理（特别是Java软件）及自动构建工具。
---

## Maven简介

Maven是一款服务于 Java 平台的自动化构建工具。



### 目前存在的问题

1. 一个项目就是一个工程

   如果项目非常庞大，就不适合继续使用package来划分模块。最好每个模块对应一个工程，利于分工合作。

   借助Maven，就可以将一个项目拆分成多个工程。

2. 项目中需要的jar包必须手动”复制粘贴“到 ***WEB-INF/lib*** 目录下

   带来的问题是，同样的jar包文件重复出现在不同的项目工程中，不仅浪费存储空间，也让工程变得臃肿。

   借助Maven，可以将jar包仅仅保存在“仓库”中，有需要使用的工程“引用”这个文件接口，而不需要真正把jar包复制过来。

3. jar包需要别人替我们准备好，或到官网下载

   借助Maven，能以一种规范的方式下载jar包，所有的知名框架或第三方工具的jar包以统一的规范存放在Maven的中央仓库中。以规范的方式下载的jar包，内容也是可靠的。

   **“统一的规范”非常重要！**

4. 一个jar包依赖的其他jar包需要自己手动加入到项目中

   如果所有的jar包之间依赖关系都需要程序员自己非常了解，就会极大增加学习成本。

   借助Maven，jar包会被自动导入。

### 构建？

[1]概念：以”Java源文件“、“框架配置文件”等资源为原材料，去生产一个可运行项目的过程。

* 编译
* 部署
* 搭建

[2]编译：Java源文件 -> 编译 -> Class字节码文件 -> JVM

[3]部署：一个B/S项目最终运行的并不是动态web工程本身，而是此web工程“编译的结果”

**在开发过程中，所有的”路径“都是以编译结果的目录结构为标准**

以Eclipse为例

![0tEgnx.jpg](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150717.png)

### 构建的各个环节

①清理：删除以前的编译结果，为重新编译做好准备。
②编译：将 Java 源程序编译为字节码文件。
③测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。
④报告：在每一次测试后以标准的格式记录和展示测试结果。
⑤打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java 工程对应 jar 包，Web工程对应 war 包。
⑥安装：在 Maven 环境下特指将打包的结果——jar 包或 war 包安装到本地仓库中。
⑦部署：将打包的结果部署到远程仓库或将 war 包部署到服务器上运行。



## Maven核心概念

①POM
②约定的目录结构
③坐标
**④依赖**
⑤仓库
⑥生命周期
⑦插件和目标
⑧继承
⑨聚合 



## Maven工程

### 创建约定的目录结构

![0tZKJg.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150717.png)

### 为什么要遵循约定的目录结构？

* Maven要负责此项目的自动化构建，必须知道Java源文件保存在何处

* 如果我们需要进行自定义，有两种方法

  * 以配置的方式明确告诉框架
  * 遵守框架内部已存在的约定

* 现在 JavaEE 开发领域普遍认同一个观点：

  **约定>配置>编码**

  意思就是能用配置解决的问题就不编码，能基于约定的就不进行配置。

  而 Maven 正是因为指定了特定文件保存的目录才能够对我们的 Java 工程进行自动化构建。

  

## 常用Maven命令

注意：**执行与构建过程相关的Maven命令，必须进入pom.xml所在的目录**

与构建过程相关：编译、测试、打包等

1. ***mvn clean*** ：清理
2. ***mvn compile*** ：编译主程序
3. ***mvn test-complie*** ：编译测试程序
4. ***mvn test*** ：执行测试
5. ***mvn package*** ：打包
6. ***mvn install*** ：安装
7. ***mvn site*** ：生成站点



## Maven联网

Maven 的核心程序中仅仅定义了抽象的生命周期，而具体的操作则是由 Maven 的插件来完成的，但Maven 的插件并不包含在 Maven 的核心程序中，在首次使用时需要联网下载。
下载得到的插件会被保存到本地仓库中。本地仓库默认的位置是：***C:\Users\user\\.m2\repository***。



## POM

Project Object Model：项目对象模型

即：将 Java 工程的相关信息封装为对象作为便于操作和管理的模型

**pom.xml是Maven 工程的核心配置文件，可以说学习 Maven 就是学习 pom.xml 中的配置**



## 坐标

* 几何中的坐标
  [1]在一个平面中使用 x、y 两个向量可以唯一的确定平面中的一个点。
  [2]在空间中使用 x、y、z 三个向量可以唯一的确定空间中的一个点。
* Maven 的坐标
  使用如下三个向量在 Maven 的仓库中唯一的确定一个 Maven 工程。
  [1]***groupid***：公司或组织的域名倒序+当前项目名称
  [2]***artifactId***：模块名
  [3]***version***：版本

```java
<groupId>com.atguigu.maven</groupId>
<artifactId>Hello</artifactId>
<version>0.0.1-SNAPSHOT</version>
```

* 如何根据坐标在仓库中找到jar包？

  [1]将 gav 三个向量连起来

  ***com.atguigu.maven+Hello+0.0.1-SNAPSHOT***

  [2]以连起来的字符串作为目录结构到仓库中查找

  ***com/atguigu/maven/Hello/0.0.1-SNAPSHOT/Hello-0.0.1-SNAPSHOT.jar***

  注意：我们自己的 Maven 工程必须执行安装操作才会进入仓库。安装的命令是：***mvn install***

## 依赖

Maven解析依赖信息时会到本地仓库中查找被依赖的jar包

对于我们自己开发的Maven工程，使用***mvn install***命令安装后就可以进入仓库

### 依赖的范围

![0tmN24.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150717.png)



![0tmUxJ.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150806.png)

### 依赖的传递性

![0tm7i8.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150757.png)

好处：可以传递的依赖不必在每个模块工程中重复声明

注意：非compile范围的依赖不能传递

### 依赖的排除

![0tmzd0.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150812.png)

设置方式：

```java
<exclusions> 
	<exclusion> 
 		<groupId>commons-logging</groupId> 
		<artifactId>commons-logging</artifactId> 
	</exclusion> 
</exclusions>
```

### 依赖的原则

作用：**解决jar包冲突问题**

1. 路径最短者优先

   ![0tncT0.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150847.png)

   

2. 路径相同时先声明者优先

   ![0tnRYT.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202150847.png)

   这里“声明”的先后顺序是指***dependency***标签配置的先后顺序

### 统一管理依赖的版本

统一声明版本号

```
<properties>
	<atguigu.spring.version>4.1.1.RELEASE</atguigu.spring.version>
</properties>
```

引用前面声明的版本号

```
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
		<version>${atguigu.spring.version}</version>
	</dependency>
	
	……

</dependencies>
```

其他用法

```
<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```



## 仓库

### 仓库的分类

1. 本地仓库：当前电脑上部署的仓库目录

2. 远程仓库：

   ​	私服：搭建在局域网环境中，为局域网范围内的所有Maven工程服务

   ​	中央仓库：架设在Internet上，为世界范围内Maven工程服务

   ​	中央仓库镜像：为了分担中央仓库的流量，提升用户访问速度

### 仓库中保存的内容

Maven工程。

1. Maven自身所需要的插件
2. 第三方框架或工具的jar包
3. 自己开发的Maven工程



## 生命周期

* Maven 生命周期定义了各个构建环节的执行顺序，如此才能自动化执行构建命令。

1. 不能打乱顺序，必须按照指定的正确顺序来执行
2. Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的
3. Maven的核心程序为了更好实现自动化构建，**不论要执行生命周期哪一阶段，都是从生命周期的最初位置开始执行**



* Maven 有三套相互独立的生命周期，分别是：

①***Clean Lifecycle*** 在进行真正的构建之前进行一些清理工作。
②***Default Lifecycle*** 构建的核心部分，编译，测试，打包，安装，部署等等。
③***Site Lifecycle*** 生成项目报告，站点，发布站点。



## 插件和目标

Maven 的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的。
每个插件都能实现多个功能，每个功能就是一个插件目标。
Maven 的生命周期与插件目标相互绑定，以完成某个具体的构建任务。



## 继承

* 情景

  Hello依赖的junit：4.0

  HelloFriend依赖的junit：4.0

  MakeFriend依赖的junit：4.9

  由于test范围得到依赖不能传递，必然会分散在各个模块工程中，容易造成版本不一致。

* 需求

  统一管理各个模块工程中对junit依赖的版本

* 解决方案

  将junit依赖统一提取到“父工程”中，在子工程中声明junit依赖时不指定版本，以父工程为准

* 步骤

  1. 创建父工程，与创建一般的 Java 工程操作一致，注意：**打包方式设置为 pom**
  2. 在子工程中引用父工程
  3. 将子工程坐标中与父工程坐标重复内容删除
  4. 在父工程中管理依赖
  5. 在子工程中删除junit依赖的版本号

* 注意

  配置继承后，执行安装命令时要先安装父工程



## 聚合

* 作用

  一键安装各个模块工程

* 配置方式

  在一个“总聚合工程”中配置各个参与聚合的模块

  ```java
  <modules>
  	<module>../Hello</module>
  	<module>../HelloFriend</module>
  	<module>../MakeFriends</module>
  </modules>
  ```

