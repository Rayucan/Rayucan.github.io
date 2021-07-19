---
title: SpringCloud-zookeeper整合时报错
date: 2021-02-06 15:05:23
tags: SpringCloud
categories: 问题解决
---

![image-20210206150634583](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210206150634.png)

<!-- more -->

**原因：虚拟机centos中防火墙未关闭**

关闭防火墙：

```
systemctl stop firewalld
```

查看防火墙状态：

```bash
systemctl status firewalld
```

![image-20210206151010080](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210206151010.png)