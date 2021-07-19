---
title: Win10解决端口占用
date: 2021-02-01 15:55:13
tags: System
categories: 问题解决
---

* win+R 打开cmd命令行窗口

* 查看端口

  ```
  netstat -aon|findstr "xxxx(端口号)"
  ```

* 查看对应PID

  ![yZ5vyn.png](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210209161002.png)

* 关闭PID对应进程

  ```
  taskkill -PID xxxx(PID) -F
  ```

