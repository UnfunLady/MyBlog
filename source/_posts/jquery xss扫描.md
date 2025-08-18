---
title: "jquery XSS扫描找出具体错误"
catalog: true
date: 2024-01-08 12:11:54
subtitle: "jquery低版本 XSS扫描"
header-img:
tags:
  - jquery
  - xss
  - nessus
catagories:
  - JavaScript
---

### 笔记描述

> 项目扫描使用了低版本 jquery 有 xss 风险 但是找遍了前端根本没发现用到了 后面学会了使用这个扫描网站获取漏洞 方便定位

### 主要代码

> 教程
https://blog.csdn.net/2401_84618514/article/details/147409397

```
输入需要扫描的ip地址 然后等待结果 
最后发现原来是后端的swagger ui使用到了  


```

### 部分截图

<img src="../jquery xss扫描/1.png"  width="600"/>
<img src="../jquery xss扫描/2.png"  width="600"/>
