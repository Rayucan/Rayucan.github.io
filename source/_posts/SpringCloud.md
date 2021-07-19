---
title: SpringCloud
date: 2021-02-21 16:52:51
tags: SpringCloud
categories: 笔记
description: 一套完整的微服务解决方案。
---

## SpringCloud概述

> SpringCloud 是微服务一站式服务解决方案，即"微服务全家桶"。
>
> 它是微服务开发的主流技术栈，且采用名称而非数字版本号。
>
> **SpringCloud** 和 **SpringCloud Alibaba** 目前是最主流的微服务框架组合。

![image-20210221171152944](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221171153.png)

### SpringCloud官方文档

SpringCloud官网：https://spring.io/projects/spring-cloud/

SpringCloud中文文档：https://www.bookstack.cn/read/spring-cloud-docs/docs-index.md

![image-20210221170537266](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221170537.png)

### SpringCloud与SpringBoot依赖关系

![image-20210221170804145](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221170804.png)

### SpringCloud总体架构与组件

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221173137.jpg)

> Eureka 就是一个服务注册与发现框架，这也是分布式微服务的基础
> Ribbon 是客户端负载均衡之组件，nginx是将所有请求都集中起来，然后再进行负载均衡
> Open Feign 是负载均衡+远程调用，即Ribbon+RestTemplate
> Hystrix 总体来说就是一个能进行熔断和降级的库，通过使用它能提高整个系统的弹性
> Zuul 微服务网关，Router and Filter : Zuul
> Config 分布式配置中心
> Bus 消息总线

## Eureka服务注册与发现

### Eureka基础知识

#### 服务治理

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221173704.jpg)

#### 服务注册

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221173907.jpg)

------

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221174010.jpg)

#### Eureka两个组件

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221174157.jpg)

### 单机Eureka

1. EurekaServer端服务注册中心类似物业公司

   ![image-20210221175041538](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175041.png)

2. EurekaClient端cloud-provider-payment8001将注册进EurekaServer成为服务提供者provider，类似学校对外提供授课服务

   ![image-20210221175114629](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175114.png)

3. EurekaClient端cloud-consumer-order80将注册进EurekaServer成为服务消费者consumer,类似上课消费的各位同学

   ![image-20210221175215452](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175215.png)

4. Eureka自我保护机制

   ![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221174542.jpg)

### 集群Eureka

#### 原理

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221174801.jpg)

#### 集群环境构建

1. ![image-20210221175446950](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175446.png)

2. 将支付服务8001微服务发布到上面2台Eureka集群配置中

3. 将订单服务80微服务发布到上面2台Eureka集群配置中

4. 测试

   ![image-20210221175701006](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175701.png)

5. 支付服务提供者8001集群环境构建

   ![image-20210221175857042](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175857.png)

6. 负载均衡

   ![image-20210221175938629](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175938.png)

7. 测试

   ![image-20210221175955580](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221175955.png)

#### actuator微服务信息完善

![image-20210221180554850](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221180554.png)

#### 服务发现Discovery

![image-20210221180643610](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221180643.png)

#### Eureka自我保护

##### 故障现象

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210221180937.jpg)

##### 导致原因

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110353.jpg)

* 某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存
* 属于CAP里面的AP分支

##### 如何禁止

![image-20210223110459528](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110459.png)

## Zookeeper服务注册与发现

### 注册中心Zookeeper

![image-20210223110600721](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110600.png)

### 服务提供者

![image-20210223110649798](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110649.png)

### 服务消费者

![image-20210223110725969](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110726.png)

## Consul服务注册与发现

### 官方文档

https://www.consul.io/intro/index.html

### Consul简介

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223112523.jpg)

![image-20210223110855199](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110855.png)

### 服务提供者

![image-20210223110917667](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110917.png)

### 服务消费者

![image-20210223110930093](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223110930.png)

### 三个注册中心异同点

#### CAP理论

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223112642.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223112822.jpg)

![image-20210223111023830](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223111023.png)

#### AP(Eureka)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223111149.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223111240.jpg)

#### CP(Zookeeper/Consul)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223111635.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223111757.jpg)

## Ribbon负载均衡服务调用

### 官方文档

https://github.com/Netflix/ribbon/wiki/Getting-Started

### Ribbon简介

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113008.jpg)

> 一句话：负载均衡+RestTemplate调用

#### LB（负载均衡）

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113140.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223112107.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223112222.jpg)

### Ribbon负载均衡演示

#### 架构说明

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113308.jpg)

> Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例

#### Template的使用

https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html![image-20210223113410838](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113410.png)

##### getForObject方法/getForEntity方法

![image-20210223113446158](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113446.png)

##### postForObject/postForEntity

![image-20210223113508365](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113508.png)

##### Get请求方法

##### POST请求方法

### Ribbon核心组件IRule

#### IRule:根据特定算法从服务列表中选取一个要访问的服务

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113646.jpg)

![image-20210223113707367](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113707.png)

#### 如何替换

![image-20210223113733362](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113733.png)

### Ribbon负载均衡算法

#### 原理

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113828.jpg)

#### 尝试自写一个本地负载均衡器

![image-20210223113914260](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223113914.png)

## OpenFeign服务接口调用

### 概述

![image-20210223114036047](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114036.png)

> Feign是一个声明式的web服务客户端，让编写web服务客户端变得非常容易，只需创建一个接口并在接口上添加注解即可

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114147.jpg)

### Feign和OpenFeign两者区别

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114222.jpg)

### OpenFeign使用步骤

![image-20210223114333408](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114333.png)

### OpenFeign超时控制

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114611.jpg)

![image-20210223114405895](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114405.png)

### OpenFeign日志打印功能

![image-20210223114421254](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114421.png)

## Hystrix断路器

### 概述

#### ![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114744.jpg)

![image-20210223114828845](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114829.png)

![image-20210223114842868](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114842.png)

![image-20210223114849250](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223114849.png)

### Hystrix简介

https://github.com/Netflix/Hystrix/wiki/How-To-Use

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115223.jpg)

![image-20210223115302967](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115303.png)

### Hystrix重要概念

#### 服务降级

![image-20210223115517064](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115517.png)

#### 服务熔断

![image-20210223115533839](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115533.png)

#### 服务限流

![image-20210223115550590](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115550.png)

### Hystrix案例

#### 构建

![image-20210223115628955](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115629.png)

#### 高并发测试

![image-20210223115723120](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115723.png)

#### 故障和导致原因

![image-20210223115749551](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115749.png)

#### 结论

![image-20210223115839142](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115839.png)

#### 如何解决

![image-20210223115855585](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223115855.png)

#### 服务降级

![image-20210223120142709](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223120142.png)

> 目前问题：
>
> 1. 每个业务方法对应一个兜底的方法，代码膨胀
> 2. 统一和自定义的分开

![image-20210223120240446](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223120240.png)

#### 服务熔断

##### 断路器

> 一句话：家里的保险丝

##### 熔断

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134317.jpg)

##### 实操

![image-20210223134346280](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134346.png)

##### 原理/总结

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134435.jpg)

![image-20210223134528641](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134528.png)

#### 服务限流

> SpringCloudAlibaba中的Sentinel再行说明。

### Hystrix工作流程

![image-20210223134658040](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134658.png)

### 服务监控HystrixDashboard

#### 概述

![image-20210223134724307](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134724.png)

#### 仪表盘9001

![image-20210223134746026](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134746.png)

#### 断路器演示

![image-20210223134841352](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223134841.png)

## ~~zuul路由网关（弃用）~~

## Gateway新一代网关

### 官方文档

https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/

### 概述

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135122.jpg)



![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135154.jpg)

> 一句话：Spring Cloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架

![image-20210223135250554](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135250.png)

### 微服务架构中网关的地位

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135341.jpg)

### 为什么弃用Zuul，选择Gateway

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135528.jpg)

### Gateway特性

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135701.jpg)

### Gateway与Zuul的区别

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135805.jpg)

### Webflex框架

![image-20210223135846045](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223135846.png)

### 三大核心概念

#### Route(路由)

> 路由是构建网关的基本模块，它由ID，目标URI，一系列的断言和过滤器组成，如果断言为true则匹配该路由

#### Predicate(断言)

> 参考的是java8的java.util.function.Predicate开发人员可以匹配HTTP请求中的所有内容（例如请求头或请求参数），如果请求与断言相匹配则进行路由

#### Filter(过滤)

> 指的是Spring框架中GatewayFilter的实例，使用过滤器，可以在请求被路由前或者之后对请求进行修改。

#### 总体

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140208.jpg)

### Gateway工作流程

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140311.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140358.jpg)

### 入门配置

![image-20210223140450793](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140450.png)

### 通过微服务名实现动态路由

![image-20210223140537892](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140537.png)

### Predicate的使用

![image-20210223140627351](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140627.png)

![image-20210223140637366](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140637.png)

#### 常用的Route Predicate

![image-20210223140717069](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140717.png)

### Filter的使用

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140917.jpg)

![image-20210223140955466](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223140955.png)

## SpringCloud Config分布式配置中心

### 分布式系统面临的问题

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223190926.jpg)

### 概述

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191020.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191203.jpg)

![image-20210223191222410](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191222.png)

### 与GitHub的整合配置

> SpringCloud Config默认使用Git来存储配置文件
>
> (也有其它方式，比如支持svn和本地文件，但最推荐的还是Git，而且使用的是http/https访问的形式)

### Config服务端配置与测试

![image-20210223191550495](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191550.png)

### Config客户端配置与测试

![image-20210223191652766](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191652.png)

> 关于bootstap.yml

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191738.jpg)

### Config客户端之动态刷新

![image-20210223191823884](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223191823.png)

## SpringCloud Bus 消息总线

### 概述

>分布式自动刷新配置功能
>
>Spring Cloud Bus配合Spring Cloud Config使用可以实现配置的动态刷新

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192027.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192103.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192135.jpg)

### RabbitMQ环境配置

![image-20210223192204500](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192204.png)

### SpringCloud Bus动态刷新全局广播

![image-20210223192302412](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192302.png)

### SpringCloud Bus动态刷新定点通知

![image-20210223192405958](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192406.png)

> 通知总结

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192449.jpg)

## SpringCloud Stream消息驱动

### 消息驱动概述

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223192820.jpg)

> 一句话：屏蔽底层消息中间件的差异，降低切换版本，统一消息的编程模型

### 设计思想

![image-20210223193623391](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193623.png)

#### 标准MQ

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193011.jpg)

![image-20210223193025492](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193025.png)

#### 为什么用Stream

![image-20210223193058391](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193058.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193223.jpg)

![image-20210223193234064](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193234.png)

#### 为什么Stream可以统一底层差异

![image-20210223193309236](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193309.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193349.jpg)

#### Binder

![image-20210223193412593](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193412.png)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193454.jpg)

> INPUT对应于消费者
>
> OUTPUT对应于生产者

### Spring Cloud Stream标准流程套路

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193712.jpg)

![image-20210223193720265](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193720.png)

![image-20210223193730827](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193730.png)

### 编码API和常用注解

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193820.jpg)

### 案例说明

![image-20210223193846051](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193846.png)

### 消息驱动之生产者

![image-20210223193910184](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193910.png)

### 消息驱动之消费者

![image-20210223193931380](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223193931.png)

### 分组消费与持久化

![image-20210223194052511](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194052.png)

## SpringCloud Sleuth分布式请求链路追踪

### 概述

> Spring Cloud Sleuth提供了一套完整的服务跟踪的解决方案
>
> 在分布式系统中提供追踪解决方案并且兼容支持了zipkin

#### 需要解决的问题

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194241.jpg)

#### 如何解决

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194423.jpg)

### 搭建链路监控

#### zipkin

> 完整的调用链路

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194709.jpg)

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194813.jpg)

> Trace:类似于树结构的Span集合，表示一条调用链路，存在唯一标识
>
> span:表示调用链路来源，通俗的理解span就是一次请求信息

#### 服务提供者

![image-20210223194905129](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194905.png)

#### 服务消费者（调用方）

![image-20210223194921837](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223194921.png)

#### 原理

![Image](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210223195014.jpg)