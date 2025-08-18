---
title: "APP端跳转第三方应用"
catalog: true
date: 2024-10-09 21:12:34
subtitle: "跳转应用"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 APP 端点击时跳转其他应用 app

### 主要代码

```js
// 高德和百度地图
		nametoMapAPP(latitude, longitude, name, index) {
				let url = '';
				if (plus.os.name == 'Android') {
					//判断是安卓端
          // 判断是否安装app
					var hasBaiduMap = plus.runtime.isApplicationExist({
						pname: 'com.baidu.BaiduMap',
						action: 'baidumap://'
					});
					var hasAmap = plus.runtime.isApplicationExist({
						pname: 'com.autonavi.minimap',
						action: 'androidamap://'
					});

					switch (index) {
						case 1:
							if (!hasAmap) {
								uni.showToast({
									title: "本机未安装高德地图应用",
									icon: "none",
									mask: true
								})
								return;
							}

							url =
								`androidamap://viewMap?sourceApplication=appname&poiname=${name}&lat=${latitude}&lon=${longitude}&dev=0`;
							break;
						case 2:
							if (!hasBaiduMap) {
								uni.showToast({
									title: "未安装百度地图应用",
									icon: "none",
									mask: true
								})
								return;
							}
							url =
								`baidumap://map/marker?location=${latitude},${longitude}&title=${name}&coord_type=gcj02&src=andr.baidu.openAPIdemo`;
							break;

						default:
							break;
					}
					if (url != '') {
						plus.runtime.openURL(url, function(e) {});
					}
				}
			}
```
