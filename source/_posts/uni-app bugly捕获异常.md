---
title: "uniapp app端对接bugly"
catalog: true
date: 2024-03-27 16:44:10
subtitle: "对接腾讯bugly"
header-img:
tags:
  - Bugly
  - uni-app
  - 插件
catagories:
  - uni-app
---

### 需求前置

`当时由于客户有很多不同手机型号 有的手机报错不知道什么问题就对接 bugly 到 uniapp 取捕获异常
但是由于 bugly 官方的是对接原生安卓的 因此我去看了 uniapp 官方原生插件的开发相关文档 我不会安卓原生 用拙劣的水平写了一个 bugly 原生插件对接`

- 我已经发布到 uniapp 插件市场让其他开发者免费使用
- https://ext.dcloud.net.cn/plugin?id=19115

### 前置步骤

在需要使用插件的页面加载以下代码
`const buglyModule = uni.requireNativePlugin('ZS-Bugly');`

### 使用方法

- 用法一:只需要传入 APPID 自动获取 APP 版本号 (推荐)

```
buglyModule.initBuglyNoVersion('上文提到的产品APPID', (res) => {
uni.showToast({ icon: 'none', title: JSON.stringify(res) });
});
```

- 用法二: 需要传入版本号和 APPID

```
buglyModule.initBugly('app版本号', 'bugly申请的产品APPID', (res) => {
uni.showToast({icon: 'none',title: JSON.stringify(res)});
});
```

### 部分截图

<img src="../uni-app bugly捕获异常/1.png" width="600" />
<img src="../uni-app bugly捕获异常/2.png" width="600" />
