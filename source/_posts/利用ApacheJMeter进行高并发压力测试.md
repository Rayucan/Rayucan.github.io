---
title: 利用Apache JMeter进行高并发压力测试
date: 2021-02-08 23:00:36
tags: Web
categories: 笔记
---

学习SpringCloud过程中使用到一个工具Apache JMeter，简单记录一下使用过程

<!-- more -->

## 介绍

> Apache JMeter是一款可以进行配置和执行负载测试、性能测试和压力测试的工具。

* 负载测试

  这类测试使系统或者应用程序在预先设计好的极端场景下测试运行，这类测试用来评估系统或者程序在极端条件下的行为。

* 性能测试

  这种测试被用来检测系统的性能表现，包括特定情况下，系统的响应能力和稳定性。

* 压力测试

  这类测试通过载入更多的外部资源，并使系统组件超越其所设定的能力范围，试图使系统挂掉。



## JMeter的简单使用

1. 创建一个线程组

   ![image-20210208230840456](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210208230840.png)
   
2. ![image-20210208231036145](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210208231036.png)

3. 创建Http请求

   ![image-20210208231131595](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210208231131.png)

4. ![image-20210208231413002](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210208231413.png)

5. 启动

   ![image-20210208231445042](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210208231445.png)