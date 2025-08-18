---
title: "sha256js获取文件的hash"
catalog: true
date: 2025-03-09 12:14:45
subtitle: "获取hash"
header-img:
tags:
  - sha256
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 sha256js 获取文件的 hash 值 不原件存储

### 主要代码

```js
<script src="/sha256.min.js"></script>;
const fingerPrint = "";
const reader = file.stream().getReader();
let hash = sha256.create();
const read = function () {
  reader
    .read()
    .then(({ done, value }) => {
      if (done) {
        fingerPrint = hash.hex();
        return;
      }
      hash.update(value);
      read();
    })
    .catch((e) => {
      console.error("文件读取过程中发生错误:", e);
    });
};
read();
```
