---
title: "[快日报]我的uni-app项目"
catalog: true
date: 2023-03-01 14:22:12
subtitle: "实习之余学习的作品."
header-img: "redian.jpg"
tags:
- uni-app
- uView
catagories:
- uniapp
---
## 编码语言
> 该项目是新闻类App项目 主要使用的编程语言是 [Vue.js](https://github.com/vuejs/vue.git), 小部分组件库使用了 [uView](https://www.uviewui.com/components/intro.html) 开发 简化了部分操作 但大部分还是用View+Flex布局实现的
---


## 项目介绍
---

使用 uniapp 开发的新闻类 app 是以数据显示核心业务为用户提供最新热点动态，让用户能及时了解世界行情。该项目前端由0到1由我独立完成
>技术栈: Vue.js、Vuex、Hbuilder X、uni-app、Echarts、uview

>职责描述:
1.vue+uview 组件开发封装unirequest 请求获取数据
2.编写工具类对返回信息格式处理展示
3.自定义tabbar对底部模块封装在需要展示tabbar的地方展示
4.对获取到的数据进行处理，封装组件展示信息 区分图片、视频格式展示不同 Card 界面
5.实现上拉刷新及下拉加载功能，并对高频功能防抖
6.使用vuex对将关键信息持久化处理并使用Echarts对信息进行可视化处理
## 初始化
---
```bash
git clone https://github.com/UnfunLady/uni-app.git ./unitest
cd unitest
npm install
```

## 运行环境(参考)
>Nodejs-version:**v14.17.3**、Npm-version:**v6.14.13**、Hbuilder X编译器!


## 运行步骤

```bash
使用 `Hbuilder X ` 打开项目
点击运行到浏览器或者真机调试
真机调试需要注意开启开发中工具的 usb调试
并且将跨域删除 接口改为真实地址！
```


## 项目部分截图
<div style="display:flex;width:100%;flex-wrap:wrap">
<img src="../hexo-theme-beantech/1.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/2.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/3.png"  width="300px"/>
<img src="../hexo-theme-beantech/4.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/5.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/6.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/7.jpg"  width="300px"/>
<img src="../hexo-theme-beantech/8.jpg"  width="300px"/>

</div>



## 结束 
---
<!-- Place this tag in your head or just before your close body tag. -->
<script async defer src="https://buttons.github.io/buttons.js"></script>
<!-- Place this tag where you want the button to render. -->

如果喜欢的话请给我的项目 <a class="github-button" href="https://github.com/UnfunLady/uni-app.git" data-icon="octicon-star" aria-label="Star UnfunLady/uni-app on GitHub">Star~

