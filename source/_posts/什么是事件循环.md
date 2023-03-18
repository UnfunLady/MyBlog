---
title: "什么是事件循环"
catalog: true
date: 2022-11-21 22:13:32
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
> JavaScript是一门单线程的语言，意味着同一时间内只能做一件事，但是这并不意味着单线程就是阻塞，而实现单线程非阻塞的方法就是事件循环

### 详细描述
> 在JavaScript中，所有的任务都可以分为
    >同步任务：立即执行的任务，同步任务一般会直接进入到主线程中执行
    >异步任务：异步执行的任务，比如ajax网络请求，setTimeout定时函数等

在执行任务时候，同步任务进入主线程，即主执行栈，异步任务进入任务队列，主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。上述过程的不断重复就事件循环

任务队列分为`宏任务`、`微任务`
>宏任务:
script (可以理解为外层同步代码)
setTimeout/setInterval
UI rendering/UI事件
postMessage、MessageChannel
setImmediate、I/O（Node.js）

微任务：
Promise.then
MutaionObserver
Object.observe（已废弃；Proxy 对象替代）
process.nextTick（Node.js）


执行一个宏任务，如果遇到微任务就将它放到微任务的事件队列中
当前宏任务执行完成后，会查看微任务的事件队列，然后将里面的所有微任务依次执行完
再执行同步任务重复这个过程 称为事件循环
---



