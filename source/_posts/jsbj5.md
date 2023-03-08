---
title: "JS统计出现最多的字符"
catalog: true
date: 2022-06-02 18:51:32
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  统计字符串中出现最多的字符个数以及字符

### 详细代码
 ```JavaScript
// 第一种
function countStr(str) {
  const obj = {}
  for (let i = 0; i < str.length; i++) {
    // 获取每个字符串
    const t = str.charAt(i)
     // 如果obj对象有该字符串 则加1
    if (obj[t]) {
      obj[t]++
    } else {
      // 没有则赋值位1
      obj[t] = 1
    }
  }
  let maxCount = 0, maxStr = ""
  for (let i in obj) {
    // 判断最大的数字及出现最多
    if (maxCount < obj[i]) {
      maxCount = obj[i];
      maxStr = i
    }
  }
}

// 第二针 for in 字符串 判断是否存在 不存在添加 存在+1
countStr("abaaabc")



 ```




