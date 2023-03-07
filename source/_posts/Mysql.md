---
title: "Mysql踩坑记录"
catalog: true
date: 2022-08-05 17:28:18
subtitle: "触发器版本要求."
header-img: 
tags:
- MySql
catagories:
- MySql
---
## 踩坑1

>在写后台项目存储公告信息时候，用varchar存储长字符串超过255报错，需要将varchar改为longtext格式

---

---
## 踩坑2

>在写后台项目数据联动的时候，用5.5版本的mysql编写触发器会失效，原因是低版本不支持多个触发器 需要更新版本！

---


