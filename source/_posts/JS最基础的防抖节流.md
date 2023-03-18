---
title: "JS最基础的防抖节流"
catalog: true
date: 2022-04-03 21:01:48
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  是最基本的防抖节流方法  能减少请求次数
### 详细代码
 ```function debounce(fn, wait) {
    let timer = null;
    return args => {
      //console.log(args);
      if (timer) clearTimeout(timer)
      timer = setTimeout(fn, wait)
    }
  }
  
  function throttle(fn, wait, arguments) {
    let time = 0;
    return function () {
      const now = new Date().getTime();
      if (now - time > wait) {
        console.log(arguments);
        fn()
        time = now
      }
    }
  }
 ```

---



