---
title: "判断是否JSON格式"
catalog: true
date: 2025-02-08 11:32:14
subtitle: "判断JSON"
header-img:
tags:
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于判断输入的是否是json格式数据
> 支持判断 带括号和不带括号格式 {"A":"B","C":"D"}  "A":"B","C":"D"

### 主要代码

```js
  $global.checkJson = (str) => {
    // 去除字符串前后的空白字符
    const trimmed = str.trim();

    // 检查完整JSON对象格式: {"A":B,"B":C}
    const fullJsonPattern = /^\{[\s\S]*\}$/;
    // 检查JSON对象内部结构: "A":B,"B":C
    const innerJsonPattern = /^[^{}]*$/;

    // 基础的键值对格式检查正则
    const keyValuePattern = /(?:"[^"]*"|'[^']*')\s*:\s*[^,]+(?:\s*,\s*(?:"[^"]*"|'[^']*')\s*:\s*[^,]+)*$/;

    // 如果是完整JSON对象格式
    if (fullJsonPattern.test(trimmed)) {
      // 移除前后的大括号后检查内部结构
      const innerContent = trimmed.slice(1, -1).trim();
      return innerContent === '' || keyValuePattern.test(innerContent);
    }
    // 如果是内部结构格式
    else if (innerJsonPattern.test(trimmed)) {
      return trimmed === '' || keyValuePattern.test(trimmed);
    }

    return false;
  }
```
