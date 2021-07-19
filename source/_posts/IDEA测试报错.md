---
title: IDEA测试报错
date: 2021-05-10 22:22:14
tags: Web
categories: 问题解决
---

在模块的test/java中进行代码测试时，报错：

`Failed to execute goal org.codehaus.mojo:exec-maven-plugin:3.0.0:exec (default-cli) on project service_vod: Command execution failed.`

<!-- more -->

搜索资料后，得出原因是**IDEA2021与maven的兼容问题**

我测试时使用的是main方法，需要改用junit进行测试

```java
public class TestXXX{
    @Test
    public void test(){
        ...
    }
}
```

