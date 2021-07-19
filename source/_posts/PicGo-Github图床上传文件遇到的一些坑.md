---
title: PicGo-Github图床上传文件遇到的一些坑
date: 2021-02-02 14:35:10
tags: Github
categories: 问题解决
---

### 坑一：上传失败，服务端错误

**配置PicGo图床时，对应Github设置中分支名错误，提示"上传失败，服务端错误"**

原因：Github已将部分术语进行替换，其中包括分支名"main"替换掉"master"

* 修改PicGo分支名

![image-20210202144043778](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202144043.png)

### 坑二：Failed to fetch

**在Typora中设置PicGo上传图片时，提示"Failed to fetch"**

原因：PicGo Server监听端口与Typora监听端口不一致

* 修改PicGo Server监听端口

![image-20210202144621213](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202144621.png)