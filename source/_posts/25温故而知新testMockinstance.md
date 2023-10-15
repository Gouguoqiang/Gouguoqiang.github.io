---
title: 实操hexo多设备维护
tags:
  - suibi
categories:
  - 八股
keywords: 面试。
description: 类似我的草稿纸。
date: 2022-9-01 11:51:56
---

## 用到的指令
常规搭好hexo只有master分支有生成文件，使用gitPage展示生成静态网站等，缺少源代码

需要创建新分支存取整个项目

github上新建分支 hexo

找到一个新文件blogSource,clone下来，切到hexo分支

将hexo所有源代码复制到新的地方

将多出来的原分支删掉  修改gitignore

然后push上去

测试： 

创建mockInstance文件夹 clone, check到hexo

新增本文件

The above is written from mockInstance

new Instance: 果然被坑了 需要将theme里的.git去掉 这是个 -r 的操作
