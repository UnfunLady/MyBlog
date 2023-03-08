---
title: "JS基础数组扁平化"
catalog: true
date: 2022-03-12 12:43:32
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  数组里面如果有数组 可以将其拍平成一个数组 如要去重可利用Set数据结构

### 详细代码
 ```JavaScript
const arr = [1, 2, 3, 4, [5, 5, 6]]
<!--第一种方法 flat可以返回一个可遍历深度的数组 Infinity表示无限深度  -->
        console.log([...new Set(arr.flat(Infinity))]);
<!--第二种方法 递归数组元素 -->
        function bp(arr) {
            return [...new Set(arr.reduce((pre, next) => {
                return pre.concat(Array.isArray(next) ? bp(next) : next)
            }, []))]


        }
        const arr1 = bp(arr);
 ```

---



