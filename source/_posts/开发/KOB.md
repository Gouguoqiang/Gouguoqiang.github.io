---
title: KOB项目
tags:
  - SpringBoot
categories:
  - 实操
keywords: 游戏。
description: KOB项目学习笔记。
date: 2022-9-01 11:51:56
---



# 配置git与项目环境

## 划分项目: 按页面分

主页(导航不需要后端实现),  个人中心界面(user), 对战界面(pk), 排名界面(ranklist),对局列表(正在对战列表)(record)

个人中心, 个人信息的数据即可



前端:  

> 1.上部导航条, 每个页面对应的具体页面展示
>
> 2.对战界面需要生成地图, 游戏需要动画,实现gameObjeck类实现每秒渲染
>
> 4.用户登录界面
>
> 5.匹配系统, 电脑对战

后端: 

> 3.用户登录集成SpringSecurity, 实现JWT登录
>
> 6.匹配系统后端, 
>
> 7.蛇的步骤后端生成等



## 前后端分离



@Controller 与@RestController

传统

![image-20230227153729495](../images/image-20230227153729495.png)

跨域不好用, 分布式Session麻烦

JWT不要各个服务器存储 根据用户信息 用秘钥进行加密判断 JWT-TOKEN是否一样

TOKEN防窃取: 两个token(access(5min,get), refresh(14days,post)) 