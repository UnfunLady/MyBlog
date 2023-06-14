---
title: "JS判断是否移动端"
catalog: true
date: 2023-04-18 12:03:18
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  是最基本的判断是否是移动端 通过浏览器的userAgent判断
### 详细代码
 ```js
  function IsPhone() {
    var info = navigator.userAgent;
    //通过正则表达式的test方法判断是否包含“Mobile”字符串
    var isPhone = /mobile/i.test(info);
    //如果包含“Mobile”（是手机设备）则返回true
    return isPhone;
  }
 ```

---



