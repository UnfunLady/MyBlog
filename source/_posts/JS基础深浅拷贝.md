---
title: "JS基础深浅拷贝"
catalog: true
date: 2022-03-03 09:23:18
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
> 浅拷贝只是简单的复制，对对象里面的对象属性和数组属性只是复制了地址，并没有创建新的相同对象或者数组。而深拷贝是空间大小占用一样但是位置不同 修改数据不影响之前的
### 详细代码
 ```JavaScript
//  递归深拷贝
 function deepClone(obj) {
  if (obj == null || typeof obj !== "object") return obj;
  let result = obj instanceof Array ? [] : {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      result[key] = deepClone(obj[key])
    }
  }
  return result
}

// 浅拷贝
Object.assign({},obj)
 ```

---



