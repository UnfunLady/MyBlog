---
title: "JS基础驼峰字符串转换"
catalog: true
date: 2022-02-08 09:57:02
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  分割字符串对字符串进行驼峰化 

### 详细代码
 ```JavaScript
const str = "str_string"
        const arr2 = str.split("_");
        arr2.forEach((i, index) => {
            if (index === 0) return;
	//将除第一个以外的数据第一个字母大写 通过Join返回字符串 
            arr2[index] = arr2[index].charAt(0).toUpperCase() + arr2[index].substring(1)
        })
        console.log(arr2.join(""));
 ```

---



