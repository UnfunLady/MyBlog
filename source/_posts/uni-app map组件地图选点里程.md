---
title: "uni-app 拖移选点展示里程"
catalog: true
date: 2023-08-11 18:19:05
subtitle: "map组件使用"
header-img:
tags:
  - uni-app
  - 位置服务
  - 地图
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> nvue 使用 map 组件 拖移屏幕选点展示距离等

### 主要代码

`实现思路: 用户拖动屏幕时候获取中心点经纬度 获取周边poi供选点 增加点击poi移动等交互 以及搜索过滤等友好交互 选择起点终点后调用第三方计算路程接口展示`

```js
map组件的regionchange方法 当屏幕移动后调用方法
regionchange(e) {
    if (this.moving) {
        return;
    }
    if (e.type == 'end') {
        this.getPoiInfo();
    }
}
// map的context
this.mapContext = uni.createMapContext('map', this);
// 获取地图中心点经纬度
this.mapContext.getCenterLocation({
success: async (res) => {
    const location = res.latitude + ',' + res.longitude;
    try {
        // 获取poi信息
        const data = await this.getPoiList(location);
        this.poiList = [];
        if (data && data.status == 0) {
            let markers = [];
            // 增加到markers属性展示坐标点
            data.result.pois.map((item, index) => {
                markers.push({
                    id: index,
                    latitude: item.point.y,
                    longitude: item.point.x,
                    iconPath: '/static/common/img/location.png',
                    width: 24,
                    height: 30,
                    name: item.name,
                    callout: {
                        content: item.name,
                        color: '#008E84',
                        fontSize: 12,
                        borderRadius: 6,
                        padding: 10,
                        display: 'ALWAYS'
                    }
                });
            });
            // 交互相关
            setTimeout(() => {
                this.markers = markers;
                this.poiList = data.result.pois;
                this.loading = false;
                this.enableScroll = true;
                this.enableZoom = true;
                uni.hideLoading();
            }, 500);
        } else {
            this.poiList = [];
            setTimeout(() => {
                this.loading = false;
                this.enableScroll = true;
                this.enableZoom = true;
                uni.hideLoading();
            }, 500);
        }
    } catch (e) {
        this.poiList = [];
        this.loading = false;
        this.enableScroll = true;
        this.enableZoom = true;
        uni.hideLoading();
    }
},
fail: (e) => {
    uni.showToast({
        title: '获取定位经纬度失败 请重试',
        icon: 'none',
        mask: true
    });
    setTimeout(() => {
        this.loading = false;
        this.enableScroll = true;
        this.enableZoom = true;
    }, 500);
    uni.hideLoading();
}
});

```

```js
// 根据上述获取到选点的起始经纬度获取polyline画线
this.getCarLine(startStr, endStr)
  .then((data) => {
    if (data && data.status == 0) {
      const routes = data.result.routes[0];
      this.miles = (Number(routes.distance) / 1000).toFixed(1);
      // this.miles=Math.ceil(Number(routes.distance)/1000)
      let paths = [];
      let points = [];
      routes.steps.map((i) => {
        paths.push(i.path);
      });
      paths
        .toString()
        .split(";")
        .map((item) => {
          let data = item.split(",");
          points.push({
            longitude: data[0],
            latitude: data[1],
          });
        });
      // 经纬度集合
      this.polyline = [
        {
          points: points,
          color: "#31c27c",
          width: 20,
          dottedLine: true,
          arrowLine: true,
          arrowIconPath: "/static/common/img/green.png",
          borderColor: "#008e84",
          borderWidth: 2,
        },
      ];
    }
  })
  .catch((e) => {
    console.log(e);
  });
```

## 部分截图

<img src="../uni-app map组件地图选点里程/1.jpg" width="300" />
<img src="../uni-app map组件地图选点里程/2.jpg" width="300" />

