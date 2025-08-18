---
title: "uni-app 位置、wifi、蓝牙权限"
catalog: true
date: 2023-05-04 19:07:41
subtitle: "备忘"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> 用于解决操作时获取权限问题

### 主要代码

```js
// 获取安卓是否打开位置
try {
  var _this = this;
  var Context = plus.android.importClass("android.content.Context");
  let main = plus.android.runtimeMainActivity();
  let system = uni.getSystemInfoSync(); // 获取系统信息
  if (system.platform === "android") {
    // 判断平台
    var locationManager = plus.android.importClass(
      "android.location.LocationManager"
    );
    var mainSvr = main.getSystemService(Context.LOCATION_SERVICE);
    if (!mainSvr.isProviderEnabled(locationManager.GPS_PROVIDER)) {
      // 没有权限
      _this.showGps = true;
      uni.hideLoading();
    } else {
      // 有打开
    }
  }
} catch (e) {
  uni.hideLoading();
  //TODO handle the exception
}
// 跳转用户系统位置权限设置代码
var context = plus.android.importClass('android.content.Context');
var locationManager = plus.android.importClass('android.location.LocationManager');
var main = plus.android.runtimeMainActivity();
var mainSvr = main.getSystemService(context.LOCATION_SERVICE);
if (!mainSvr.isProviderEnabled(locationManager.GPS_PROVIDER)) {
    var Intent = plus.android.importClass('android.content.Intent');
    var Settings = plus.android.importClass('android.provider.Settings');
    var intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
    main.startActivity(intent); // 打开系统设置GPS服务页面
} else {
    // console.log('GPS功能已开启');
}

// 打开用户wifi方法
openWifi() {
  try {
      const MainActivity = plus.android.runtimeMainActivity();
      const Context = plus.android.importClass('android.content.Context');
      plus.android.importClass('android.net.wifi.WifiManager');
      plus.android.importClass('java.util.List');
      plus.android.importClass('java.util.ArrayList');
      plus.android.importClass('android.net.wifi.ScanResult');
      plus.android.importClass('android.net.wifi.WifiInfo');
      plus.android.importClass('java.util.BitSet');
      plus.android.importClass('android.net.wifi.WifiConfiguration');
      // wifi管理
      this.wifiManager = MainActivity.getSystemService(Context.WIFI_SERVICE);
      this.androidOpenWifi();
  } catch (e) {
      uni.hideLoading();
      //TODO handle the exception
  }
}
androidOpenWifi() {
    try {
        let bRet = false;
        const wifiManager = this.wifiManager;
        if (!wifiManager.isWifiEnabled()) {
            this.$u.toast('请打开WIFI');
            bRet = wifiManager.setWifiEnabled(true); //返回自动打开的结果
            console.log('打开wifi的返回结果是' + bRet);
        } else {
            bRet = true;
            // 打开蓝牙
            this.openBluetooth();
            console.log('wifi原本已经打开');
        }
        return bRet;
    } catch (e) {
        uni.hideLoading();
        //TODO handle the exception
    }
}

// 打开蓝牙适配器
 try {
  const permissions = {
      'android.permission.BLUETOOTH': '蓝牙权限', // 蓝牙权限
      'android.permission.BLUETOOTH_SCAN': '蓝牙扫描权限', // 蓝牙权限
      'android.permission.BLUETOOTH_ADMIN': '蓝牙管理权限', // 蓝牙权限
      'android.permission.BLUETOOTH_CONNECT': '蓝牙连接权限', // 蓝牙权限
      'android.permission.ACCESS_FINE_LOCATION': '定位位置权限', // 定位
      'android.permission.ACCESS_COARSE_LOCATION': '位置信息权限' // 定位
  };
  let _this = this;
  plus.android.requestPermissions(Object.keys(permissions), function (resultObj) {
      console.log(resultObj);
      let main = plus.android.runtimeMainActivity();
      let Context = plus.android.importClass('android.content.Context');
      let BManager = main.getSystemService(Context.BLUETOOTH_SERVICE);
      let BlueToothAdapter = plus.android.importClass('android.bluetooth.BluetoothAdapter');
      let BAdapter = BlueToothAdapter.getDefaultAdapter();
      if (!BAdapter.isEnabled()) {
          _this.$u.toast('请打开蓝牙');
      } else {
      //  打开了
      }
  });
} catch (e) {
  uni.hideLoading();
  //TODO handle the exception
}
```
