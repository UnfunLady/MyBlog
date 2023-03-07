---
title: "[易企通]我的React项目"
catalog: true
date: 2023-03-05 17:28:18
subtitle: "实习之余学习的作品."
header-img: "react.jpg"
tags:
- React
catagories:
- React
---
## 编码语言
> 该项目是后台项目 主要使用的编程语言是 [React.js](https://github.com/facebook/react), 小部分组件库使用了 [Ant-design](https://ant.design/components/overview-cn) 开发 
---


## 项目介绍
---
使用 `React` 开发的为企业管理人员设计的系统,大部分使用Antd组件库实现UI搭建,
后端采用`SpringBoot`框架实现后端接口，简化平时操作流程，可以完成绝大部分管理需求。
>技术栈: React.redux、Echarts、pub-sub、Antd、Hooks、Springboot、Mysql

>职责描述:
1.使用SpringBoot完成后端，使用JWT实现以及自定义注解拦截器实现登录Token验证用户信息
2.使用Antd组件库从0到1搭建页面布局，使用React、JSX以及TypeScript实现UI页面
3.使用redux对登录信息持久化存储，pub-sub进行组件通信，BraftEditor富文本公告及图片上传
4.自定义Hooks实现路由鉴权判断用户是否登录，并对登录用户身份鉴定，渲染不同身份的菜单
5.通过Echarts渲染企业信息及员工打卡各项信息实现可视化数据。
6.封装复用组件，对上传图片进行压缩，并对上传但未使用图片进行删除优化
## 初始化
---
```bash
git clone https://github.com/UnfunLady/reactApps.git ./reactApp
cd reactApp
npm install or npm i
将项目里的Mysql文件导入至你的数据库  并准备下载后台
```
```
git clone https://github.com/UnfunLady/reactApp_backend.git ./backend
打开项目等待依赖环境下载完毕 运行springboot项目 注意：先修改mybatisplus的配置
修改数据库名 端口号等与你的环境一致即可
```

## 运行环境(参考)
>Nodejs-version:**v14.17.3**、Npm-version:**v6.14.13**、Mysql-version:**8.0+**、springboot


## 运行步骤

```bash
1.一般使用 `IDE编辑器` 打开后台项目
2.运行项目 等待后端项目启动
3.使用`vscode or webstorm`打开前端项目 npm start
4.运行项目 等待前端项目启动
```


## 项目部分截图
<div style="display:flex;width:100%;flex-wrap:wrap">
<img src="../reactApp/one.jpg"  />
<img src="../reactApp/two.jpg"  />
<img src="../reactApp/three.jpg"  />
<img src="../reactApp/three-1.jpg"  />
<img src="../reactApp/four.jpg"  />
</div>



## 结束 
---
<!-- Place this tag in your head or just before your close body tag. -->
<script async defer src="https://buttons.github.io/buttons.js"></script>
<!-- Place this tag where you want the button to render. -->

如果喜欢的话请给我的项目 <a class="github-button" href="https://github.com/UnfunLady/reactApps.git" data-icon="octicon-star" aria-label="Star UnfunLady/uni-app on GitHub">Star~

