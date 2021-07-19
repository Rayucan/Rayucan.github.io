---
title: Nginx
date: 2021-02-20 15:03:24
tags: Web
categories: 笔记
description: 一个高性能的HTTP和反向代理web服务器。
---

## Nginx简介

Nginx 是高性能的 HTTP 和反向代理的服务器，处理高并发能力十分强大，能经受高负载的考验，支持高达 50,000 个并发连接数。

### 正向代理

需要在**客户端**配置代理服务器进行指定网站访问

![image-20210220151017173](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220151017.png)

### 反向代理

暴露的是代理服务器地址，隐藏了真实服务器 IP 地址

![image-20210220150955474](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220150955.png)

### 负载均衡

增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到**单个服务器**上的情况改为将请求分发到**多个服务器**上，将负载分发到不同的服务器，也就是我们所说的**负载均衡**

![image-20210220151109155](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220151109.png)

### 动静分离

![image-20210220151132222](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220151132.png)

## Nginx安装

https://blog.csdn.net/qq_42815754/article/details/82980326

> 查看Nginx状态

```bash
ps -ef | grep nginx
```

关于`ps -ef | grep`命令的详解https://www.cnblogs.com/freinds/p/8074651.html

> 注意先关闭Linux防火墙才能在windows浏览器中访问Nginx

```bash
systemctl stop firewalld
```

## Nginx常用命令

进入 nginx 目录中 

```bash
cd /usr/local/nginx/sbin
```

1. 查看Nginx版本号

   ```bash
   ./nginx -v
   ```

2. 启动Nginx

   ```bash
   ./nginx
   ```

3. 停止Nginx

   ```bash
   ./nginx -s stop
   ```

4. 重载Nginx

   ```bash
   ./nginx -s reload
   ```


## Nginx配置文件

### 配置文件位置

```bash
cd /usr/local/nginx/conf/nginx.conf
```

![image-20210220163942794](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220163942.png)

### 配置文件内容

1. 全局块：配置服务器整体运行的配置指令

   比如 worker_processes 1;处理并发数的配置

2. events 块：影响 Nginx 服务器与用户的网络连接

   比如 worker_connections 1024; 支持的最大连接数为 1024

3. http 块： Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里

   需要注意的是：**http 块**也可以包括 **http 全局块**、**server 块**

## Nginx原理

### master与worker

![image-20210220201254800](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220201254.png)

![image-20210220201334993](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220201335.png)

### worker如何进行工作

![image-20210220201617332](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210220201617.png)

### 一个master与多个worker

1. 可以使用 `nginx –s reload` 热部署，利用 nginx 进行热部署操作
2. 每个 woker 是独立的进程，如果有其中的一个 woker 出现问题，其他 woker 独立， 继续进行争抢，实现请求过程，**不会造成服务中断**

### worker的合适设置数量

worker 数和服务器的 cpu 数相等是最为适宜的

### 连接数 worker_connection

1. 发送请求占用了 woker 几个连接数？
   * 2 或者 4 个
2. 一个 master 四个 woker，每个 woker 支持最大的连接数 1024，支持的最大并发数是多少？
   * 普通的静态访问最大并发数是： worker_connections * worker_processes / 2
   * 而如果是 HTTP 作为反向代理来说，最大并发数量应该是 worker_connections * worker_processes / 4



