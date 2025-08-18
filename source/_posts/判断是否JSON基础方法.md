---
title: "判断是否JSON格式"
catalog: true
date: 2025-02-03 21:12:30
subtitle: "判断JSON"
header-img:
tags:
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于判断输入的是否是json格式数据

### 主要代码

```js
  $global.isJSON = (value) => {
    // 如果是字符串，尝试解析
    if (typeof value === 'string') {
      try {
        const parsed = JSON.parse(value);
        return (
          parsed !== null &&
          typeof parsed === 'object' &&
          !Array.isArray(parsed)
        );
      } catch (e) {
        return false;
      }
    }

    // 如果是对象，检查是否为普通对象
    return (
      value !== null &&
      typeof value === 'object' &&
      !Array.isArray(value) &&
      Object.getPrototypeOf(value) === Object.prototype
    );
  }
```
