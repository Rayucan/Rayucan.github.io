---
title: IDEA导入aliyun-sdk-vod-upload依赖报错
date: 2021-05-10 22:00:51
tags: Web
categories: 问题解决
---

使用阿里云视频点播功能时，service_vod模块的aliyun-sdk-vod-upload依赖一直爆红

查阅文档，发现**该依赖并未正式开源，需要自行去阿里云官网下载**

但是下载完成并通过cmd命令手动导入至maven仓库后，该依赖**仍然爆红**，让我感到十分疑惑

<!-- more -->

反复检查jar包依赖对应关系正确后，发现原来是我一直使用的是IDEA自带的maven，也就是忘改设置了...

现在换成了自己的maven，以及settings配置文件和仓库路径

**细节决定成败**。

![image-20210510220743199](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210510220750.png)

