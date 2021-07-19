---
title: Spring连接Redis报错
date: 2021-06-02 16:09:21
tags: Spring
categories: 问题解决
---

测试验证码存入redis时，spring报错：

```java
org.springframework.data.redis.connection.PoolException: Could not get a resource from the pool; nested exception is io.lettuce.core.RedisConnectionException: Unable to connect to 192.168.161.129:6379
```

<!-- more -->

原因：

虚拟机防火墙未关闭，spring无法连接redis

解决方法：

关闭虚拟机防火墙

```bash
systemctl stop firewalld
```

