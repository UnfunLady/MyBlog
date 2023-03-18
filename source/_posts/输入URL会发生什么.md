---
title: "输入URL会发生什么"
catalog: true
date: 2023-01-01 17:43:42
subtitle: "学习经验."
header-img: 
tags:
- HTTP
catagories:
- HTTP
---

### 笔记描述
> 浏览器输入url到显示的过程会发生什么

### 详细描述
> 首先检查url格式如果不是ip地址则开启dns域名解析  
  先在浏览器缓存 系统缓存 路由器缓存 如果都没有查到就 启动dns域名解析 查找到ip地址
>三次握手建立tcp联机
 第一次握手 客户端发送 SYN=1 seq=x随机数据包 `close状态变为 syn-sent状态` 
 服务端收到客户端syn请求 同意连接 向客户端 发送 SYN=1  ACK=1 并发送ack=客服端seq+1 
 seq=随机数据表示确认连接 服务端从 `listen状态变为 syn-rcvd状态`  
 客户端收到服务器端的连接请求确认后 发送 ACK=1 seq=第一次seq+1 以及ack=服务器端seq+1 
 客户端连接已建立 `syn-send变成established`
  服务器端收到ack请求确认后 认为建立完成 `syn-rcvd变成established `

<img src="../输入URL会发生什么/three.jpg"/>


> ①  SYN(synchronous建立联机)；
②  ACK(acknowledgement 确认)
③  PSH(push传送)
④  FIN(finish结束)
⑤  RST(reset重置)
⑥  URG(urgent紧急)
⑦  Sequence number(顺序号码) //Acknowledge number(确认号码)



  紧接着

  浏览器发送HTTP请求

  HTTP响应 服务器处理请求返回数据

  浏览器解析html文件生成dom树，解析css生成css树 最后合并render树，

  浏览器计算渲染对象在可视区域的位置大小 并绘制

  tcp四次挥手断开连接

  >客户端发送FIN=1 seq=随机值 不再发送数据 但是可以接受数据  `established变成finwait1 ` 
  服务器收到请求后发送ACK=1 ack=客户端的seq+1 以及自己的seq=随机值 标识收到 但是还有数据要传输可能 服务器`CLOSE_WAIT`  
  客户端收到后 `finwait1转finwait2` 
  数据传输完毕后 服务器发送FIN=1 ACK=1 seq=随机值，ack=客户端seq+1 进入 `laskack最后确认`  
  最后客户端收到服务器请求后 发送ACK=1 seq=第一次seq+1 ack为服务器seq+1
  最后进入`Timewait` 服务器收到后`进入 close` 服务器关闭 `随后客户端等待2MSL后进入close`
    2msl是数据包来回最大时间  确保没有收到数据后 断开

   <img src="../输入URL会发生什么/four.jpg"/>
---



