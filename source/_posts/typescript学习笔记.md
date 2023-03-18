---
title: "typescript学习笔记"
catalog: true
date: 2023-01-23 22:13:08
subtitle: "学习经验."
header-img: 
tags:
- TypeScript
catagories:
- TS
---

### 笔记描述
>  typescript中type和interface的区别 在自己些项目的时候确实没考虑过

### 详细记录
* 1.type和interface的相同点：都是用来定义对象或函数的形状。
* 2.interface通过extends实现继承，type是通过&实现
  ```tsx
      type aa = {
        name: string
    }
 
    interface bb {
        name: string
    }
    
 
    type cc = aa & {
        age: number
    }
 
    type cc = bb & {
        age: number
    }
 
    interface dd extends aa {
        age: number
    }
 
    interface dd extends bb {
        age: number
    }
  ```
* 3.type可以定义 基本类型的别名，如 type myString = string interface只能定义对象{}

* 4.type可以通过 typeof 操作符来定义，如 type myType = typeof someObj
  
* 5.interface相同名字可以自动合并，如果是type的话，就会报重复定义的警告
  ```tsx
      interface test {
        name: string
    }
 
    interface test {
        age: number
    }
    
    /*
        test实际为 {
            name: string
            age: number
        }
    */
  ```




