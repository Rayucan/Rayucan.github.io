---
title: SpringCloud-OpenFeign引入后报错
date: 2021-05-22 23:28:23
tags: SpringCloud
categories: 问题解决
---

springcloud引入openfeign依赖后，启动项目报错：

```
Unsatisfied dependency expressed through field 'vodClient'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.atguigu.eduservice.client.VodClient': FactoryBean threw exception on object creation; nested exception is java.lang.IllegalStateException: No Feign Client for loadBalancing defined. Did you forget to include spring-cloud-starter-loadbalancer?
```

<!-- more -->

关键句：

`Did you forget to include spring-cloud-starter-loadbalancer?`

> 有人问我，R老师发生甚么事了？
>
> 我一看，噢，原来是昨天，我把springcloud升级到了最新版本

我大意了啊，没有闪。

查阅一番后发现：

>SpringCloud Feign在Hoxton.M2 RELEASED版本之后**不再使用Ribbon，而是使用spring-cloud-loadbalancer**，所以不引入spring-cloud-loadbalancer会报错

解决方法：

1. **加入spring-cloud-loadbalancer依赖，并且在nacos中排除ribbon依赖，不然loadbalancer无效**

   ```xml
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
               <version>2.2.5.RELEASE</version>
               <exclusions>
                   <exclusion>
                       <groupId>org.springframework.cloud</groupId>
                       <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
   
           <!-- spring-cloud-loadbalancer -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-loadbalancer</artifactId>
           </dependency>
   ```

2. 或是降低SpringCloud、SpringBoot版本
