---
title: Github-CNAME文件问题
date: 2020-12-20 15:43:17
tags: Github
categories: 问题解决
---

CNAME文件在下次 `hexo deploy`的时候就消失了，需要重新创建，如何解决？

<!-- more -->

**注意：CNAME只允许一个域名地址。**



#### 方法一

每次 `hexo d` 之后，就去 GitHub 仓库根目录新建 CNAME文件



#### 方法二

在 `hexo g` 之后， `hexo d` 之前，把CNAME文件复制到 “\public\” 目录下面，里面写入你要绑定的域名。



#### 方法三（推荐）

**将需要上传至github的内容放在 source 文件夹，例如CNAME、favicon.ico、images等，这样在 `hexo d` 之后就不会被删除了。**



#### 方法四

通过安装插件实现永久保留

```bash
$ npm install hexo-generator-cname --save
```

之后在_config.yml中添加一条

```yaml
Plugins:
- hexo-generator-cname
```

另外，如果是在github上建立的CNAME文件，需要先clone到本地，然后安装插件，在deploy上去即可。

