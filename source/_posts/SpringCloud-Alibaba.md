---
title: SpringCloud-Alibaba
date: 2021-02-24 18:49:17
tags: SpringCloud
categories: 笔记
description: 为分布式应用程序开发提供一站式解决方案。
---

## SpringCloud Alibaba概述

![image-20210224185433867](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224185440.png)

### 为什么会出现SpringCloud Alibaba

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224185906.jpg)

> SpringCloud NetFlix Projects Entering Maintenance Mode.
>
> 即Spring Cloud Netflix项目进入维护模式。

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224185803.jpg)

![image-20210224185927584](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224185927.png)

![image-20210224185939134](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224185939.png)

### SpringCloud Alibaba带来了什么

https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190046.jpg)

### SpringCloud Alibaba能做些什么

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190115.jpg)

>
> Sentinel：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。
>
> Nacos：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
>
> RocketMQ：一款开源的分布式消息系统，基于高可用分布式集群技术，提供低延时的、高可靠的消息发布与订阅服务。
>
> Dubbo：Apache Dubbo™ 是一款高性能 Java RPC 框架。
>
> Seata：阿里巴巴开源产品，一个易于使用的高性能微服务分布式事务解决方案。
>
> Alibaba Cloud ACM：一款在分布式架构环境中对应用配置进行集中管理和推送的应用配置中心产品。
>
> Alibaba Cloud OSS: 阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。
>
> Alibaba Cloud SchedulerX: 阿里中间件团队开发的一款分布式任务调度产品，提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。
>
> Alibaba Cloud SMS: 覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。

## SpringCloud Alibaba Nacos服务注册和配置中心

### Nacos简介

> 前四个字母分别为Naming和Configuration的前两个字母，最后的s为Service

![image-20210224190418405](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190418.png)

![image-20210224190454944](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190454.png)

### 各注册中心的比较

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190617.jpg)

### Nacos作为服务注册中心演示

![image-20210224190712732](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190712.png)

### Nacos作为服务配置中心演示

#### 基础配置

![image-20210224190834762](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190834.png)

> 为什么配置两个YML

![image-20210224191025228](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191025.png)

#### 分类配置

> 多环境多项目管理问题

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191326.jpg)

![image-20210224190908848](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224190908.png)

> Namespace+Group+Data ID三者关系

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191216.jpg)

![image-20210224191227526](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191227.png)

### Nacos集群和持久化配置（重要）

https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191558.jpg)

#### 官方架构图

![image-20210224191420005](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191420.png)

#### 真实情况

![image-20210224191456250](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191456.png)

![image-20210224191501244](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191501.png)

#### Nacos持久化配置解释

![image-20210224191652316](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191652.png)

#### Linux版Nacos+MySQL生产环境配置

![image-20210224191747344](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191747.png)

> 总结

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191824.jpg)

## SpringCloud Alibaba Sentinel实现熔断与限流

https://github.com/alibaba/Sentinel

### Sentinel简介

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224191958.jpg)

> 一句话：之前讲解过的Hystrix。

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192047.jpg)

> 服务使用中的各种问题

* 服务雪崩
* 服务降级
* 服务熔断
* 服务限流

### 初始化演示工程

![image-20210224192220615](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192220.png)

### 流控规则

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192405.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192332.jpg)

![image-20210224192338271](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192338.png)

#### 流控模式

![image-20210224192541176](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192541.png)

#### 流控效果

![image-20210224192609262](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192609.png)

### 降级规则

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192652.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192727.jpg)

#### RT

![image-20210224192756762](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192756.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192828.jpg)

#### 异常比例

![image-20210224192852014](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224192852.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193442.jpg)

#### 异常数

![image-20210224193504765](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193504.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193533.jpg)

### 热点Key限流

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193637.jpg)

![image-20210224193744643](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193744.png)

> 添加异常

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193811.jpg)

### 系统规则(不常用)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224193902.jpg)

### @SentinelResource注解

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194203.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194244.jpg)

![image-20210224194036952](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194037.png)

> "兜底方法"面临的问题

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194120.jpg)

### 服务熔断功能

> sentinel整合ribbon+openFeign+fallback

#### Ribbon系列

![image-20210224194457771](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194457.png)

#### Feign系列

![image-20210224194545338](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194545.png)

#### 熔断框架比较

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194624.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194644.jpg)

#### 规则持久化

![image-20210224194724554](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194724.png)

## SpringCloud Alibaba Seata处理分布式事务

### 分布式事务问题

* 分布式前

  ![image-20210224194916236](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194916.png)

* 分布式后

![image-20210224194939213](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224194939.png)

![image-20210224195019965](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195020.png)

> 一句话：一次业务操作需要跨多个数据源或需要跨多个系统进行远程调用，就会产生分布式事务问题。

### Seata简介

http://seata.io/zh-cn/

>Seata是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单易用的分布式事务服务。

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195301.jpg)

> 一个典型的分布式事务过程

![image-20210224195200954](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195201.png)

![image-20210224195216957](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195216.png)

![image-20210224195221354](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195221.png)

### 订单/库存/账户业务数据库准备

![image-20210224195355892](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195355.png)

> 业务说明

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195431.jpg)

### 订单/库存/账户业务微服务准备

![image-20210224195524443](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195524.png)

### 测试

![image-20210224195604382](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210224195604.png)

### Seata原理

https://blog.csdn.net/f4761/article/details/89077400