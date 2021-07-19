---
title: Eclipse基本操作和配置
date: 2021-06-19 23:16:09
tags: Web
categories: 笔记
---

由于学校机房的IDE只有eclipse，因而不得不在自己的电脑重新下载安装，以便熟悉eclipse开发环境~~（应付考试）~~

为了符合之前在IDEA的编码习惯，下面总结了一些eclipse的基本操作和配置

<!-- more -->

> 注意：
>
> 使用的Eclipse版本为[Eclipse IDE for Enterprise Java and Web Developers](https://www.eclipse.org/downloads/packages/release/2021-06/r/eclipse-ide-enterprise-java-and-web-developers)
>
> 也就是 J2EE 版

### 快捷键

Alt + / : 用于sout、try catch等代码生成

Ctrl + 2 + L : 快捷生成对应变量

### 生成Getter & Setter、Constructor等

空白处右键>>Source>>

![image-20210619233908225](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210619233908.png)

### 字体大小样式

工具栏Window>>Preferences>>General>>Appearance>>Colors and Fonts

展开Basic>>Text Font>>点击右侧Edit

![image-20210619232433359](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210619232433.png)

### 自动补全代码

工具栏Window>>Preferences>>Java>>Editor>>Content Assist

在红框处.号后面输入26个字母

![image-20210619232721209](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210619232721.png)

### Tomcat配置

工具栏Window>>Preferences>>Server>>Runtime Environments

点击add配置本机安装的tomcat

![image-20210619233203735](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210619233203.png)
