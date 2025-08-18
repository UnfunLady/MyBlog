---
title: "APP端离线获取经纬度"
catalog: true
date: 2024-08-27 12:42:51
subtitle: "获取经纬度"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 APP 端离线时获取经纬度

### 主要代码

```js
uni.getNetworkType({
  success: (res1) => {
    let _this = this;
    if (res1.networkType === "none") {
      // #ifdef APP
      plus.geolocation.getCurrentPosition(
        function (p) {
          try {
            const res = {
              latitude: p.coords.latitude,
              longitude: p.coords.longitude,
            };
            _this.longitude = res.longitude;
            _this.latitude = res.latitude;
            console.log("离线经纬度", _this.longitude, _this.latitude);
          } catch (e) {
            console.log(e);
            //TODO handle the exception
          }
        },
        function (e) {},
        {
          provider: "amap",
          /*
          provider: (String 类型 )优先使用的定位模块
可取以下供应者： "system"：表示系统定位模块，支持wgs84坐标系； "baidu"：表示百度定位模块，支持gcj02/bd09/bd09ll坐标系； "amap"：表示高德定位模块，支持gcj02坐标系。 默认值按以下优先顺序获取（amap>baidu>system），若指定的provider不存在或无效则返回错误回调。 注意：百度/高德定位模块需要配置百度/高德地图相关参数才能正常使用。
          */
        }
      );
      // #endif
    } else {
      // 有网络
      uni.getLocation({
        type: "gcj02",
        success: (res) => {
          this.longitude = res.longitude;
          this.latitude = res.latitude;
        },
        fail: (err) => {
          uni.showToast({
            title: err,
            icon: "none",
          });
        },
      });
    }
  },
});
```
