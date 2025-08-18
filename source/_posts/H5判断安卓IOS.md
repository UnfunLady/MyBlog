---
title: "H5判断当前是IOS还是安卓环境"
catalog: true
date: 2024-08-04 19:12:28
subtitle: "获取操作系统环境"
header-img:
tags:
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 H5 获取操作系统环境

### 主要代码

```js
function isIOS() {
  const userAgent = navigator.userAgent || navigator.vendor || window.opera;
  return /iPad|iPhone|iPod/.test(userAgent) && !window.MSStream;
}
```
