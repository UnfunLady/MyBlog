---
title: "uniapp app端打开微信小程序"
catalog: true
date: 2024-12-13 22:13:28
subtitle: "打开小程序"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 APP 端拉起微信小程序

### 主要代码

```js
plus.share.getServices(
  function (services) {
    var service = services.find(function (service) {
      return service.id === "weixin";
    });
    if (service) {
      service.launchMiniProgram({
        id: "xxx", // 替换为实际的小程序"原始id"
        success: function () {
          console.log("成功打开小程序");
        },
        fail: function (err) {
          console.error("打开小程序失败", err);
        },
      });
    } else {
      console.log("未找到微信服务");
    }
  },
  function (err) {
    console.error("获取分享服务失败", err);
  }
);
```
