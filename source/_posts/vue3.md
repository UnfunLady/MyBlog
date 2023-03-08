---
title: "Vue3项目"
catalog: true
date: 2023-03-01 13:07:08
subtitle: "学习Vue3的作品."
header-img: "vue.jpg"
tags:
- Vue
- Mysql
- Nodejs
catagories:
- Vue
---
## 编码语言
> 该项目是后台项目 主要使用的编程语言是 [Vue.js](https://github.com/vuejs/vue.git), 小部分组件库使用了 [ElementPlus](https://element-plus.gitee.io/zh-CN/component/button.html) 开发 
> 后台项目使用Nodejs的Express框架开发
---


## 项目介绍
---
使用 `Vue3` 开发的为企业管理人员设计的系统,大部分使用Element组件库实现UI搭建,
后端采用`Nodejs`框架实现后端接口，简化平时操作流程，可以完成绝大部分管理需求。
>技术栈: Vue3、pinia、Echarts、ElementPlus、Nodejs、Mysql

>职责描述:
1.使用Nodejs完成后端，使用JWT实现登录Token 自定义中间件实现Token验证
2.使用ElementPlus组件库从0到1搭建页面布局，使用Vue3以及TypeScript实现UI页面
3.使用Pinia对登录信息持久化存储。upload组件实现图片上传
4.通过Echarts渲染企业信息及员工打卡各项信息实现可视化数据。
5.封装复用组件，对上传图片进行压缩
## 初始化
---
```bash
git clone https://github.com/UnfunLady/Vue3_practice.git ./Vuets
cd Vuets
npm install or npm i
将项目里的Mysql文件导入至你的数据库  并准备运行后台
```
```
修改backend下的db文件的数据库名 端口号等与你的环境一致即可
npm start运行nodejs后台
```

## 运行环境(参考)
>Nodejs-version:**v14.17.3**、Npm-version:**v6.14.13**、Mysql-version:**8.0+**、


## 运行步骤

```bash
1.一般使用 `Vscode` 打开后台项目及前台项目
2.运行项目 等待后端项目启动 npm start
3.打开前端项目 npm run serve
4.运行项目 等待前端项目启动
```


## 项目部分截图
<div style="display:flex;width:100%;flex-wrap:wrap">
<img src="../vue3/vue1.jpg"  />
<img src="../vue3/vue2.jpg"  />
<img src="../vue3/vue3.jpg"  />
<img src="../vue3/vue4.jpg"  />
</div>



## 结束 
---
<!-- Place this tag in your head or just before your close body tag. -->
<script async defer src="https://buttons.github.io/buttons.js"></script>
<!-- Place this tag where you want the button to render. -->

如果喜欢的话请给我的项目 <a class="github-button" href="https://github.com/UnfunLady/Vue3_practice.git" data-icon="octicon-star" aria-label="Star UnfunLady/uni-app on GitHub">Star~

