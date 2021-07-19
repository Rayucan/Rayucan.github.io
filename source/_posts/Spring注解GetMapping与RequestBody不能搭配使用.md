---
title: Spring@GetMapping与@RequestBody不能搭配使用
date: 2021-06-01 23:02:10
tags: SpringCloud
categories: 问题解决
---

项目整合登录验证功能时遇到如下错误：

```java
org.springframework.http.converter.HttpMessageNotReadableException: Required request body is missing
```

<!-- more -->

错误原因：

**误把@GetMapping与@RequestBody放在一起使用**

```java
    @GetMapping("login")
    public R loginUser(@RequestBody UcenterMember member){
    }
```

解决方式：

**@GetMapping换成@PostMapping**

```java
    @PostMapping("login")
    public R loginUser(@RequestBody UcenterMember member){
    }
```



stackoverflow上的一个类似问题：

https://stackoverflow.com/questions/42256358/spring-getmapping-with-requestparam-and-requestbody-fails-with-httpmessagenot

另附GET与POST的区别：

https://blog.fundebug.com/2019/02/22/compare-http-method-get-and-post/

