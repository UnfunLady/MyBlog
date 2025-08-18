---
title: "uni-app 部分权限获取"
catalog: true
date: 2023-05-01 09:37:19
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
// 获取拨打电话权限
const phone = this.personDetail.phone;
plus.android.requestPermissions(
  ["android.permission.CALL_PHONE"],
  function (resultObj) {
    var result = 0;
    if (resultObj.granted.length == 0) {
      uni.showModal({
        content: "需先获取拨打电话权限：",
        showCancel: false,
        success() {
          //没有开对应的权限，打开app的系统权限管理页
          let Intent = plus.android.importClass("android.content.Intent");
          let Settings = plus.android.importClass("android.provider.Settings");
          let Uri = plus.android.importClass("android.net.Uri");
          let mainActivity = plus.android.runtimeMainActivity();
          let intent = new Intent();
          intent.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
          let uri = Uri.fromParts(
            "package",
            mainActivity.getPackageName(),
            null
          );
          intent.setData(uri);
          mainActivity.startActivity(intent);
        },
      });
      return;
    } else {
      // 拨打电话api
      uni.makePhoneCall({
        phoneNumber: phone,
      });
    }
  },
  function (error) {
    uni.showToast({
      title: "申请拨打电话的权限错误",
      icon: "none",
      mask: true,
      duration: 2000,
    });
  }
);

// 申请发送短信权限
const phone = this.personDetail.phone;
plus.android.requestPermissions(
  ["android.permission.SEND_SMS"],
  function (resultObj) {
    var result = 0;
    if (resultObj.granted.length == 0) {
      uni.showModal({
        content: "需先获取发送短信权限：",
        showCancel: false,
        success() {
          //没有开对应的权限，打开app的系统权限管理页
          let Intent = plus.android.importClass("android.content.Intent");
          let Settings = plus.android.importClass("android.provider.Settings");
          let Uri = plus.android.importClass("android.net.Uri");
          let mainActivity = plus.android.runtimeMainActivity();
          let intent = new Intent();
          intent.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
          let uri = Uri.fromParts(
            "package",
            mainActivity.getPackageName(),
            null
          );
          intent.setData(uri);
          mainActivity.startActivity(intent);
        },
      });
      return;
    } else {
      var msg = plus.messaging.createMessage(plus.messaging.TYPE_SMS);
      msg.to = [phone];
      msg.body = "";
      plus.messaging.sendMessage(msg);
    }
  },
  function (error) {
    uni.showToast({
      title: "申请发送短信的权限错误",
      icon: "none",
      mask: true,
      duration: 2000,
    });
  }
);

// 相机权限

plus.android.requestPermissions(["android.permission.CAMERA"], (e) => {
  if (e.deniedPresent.length > 0 || e.deniedAlways.length > 0) {
    uni.showModal({
      content: "相机权限被拒绝 请在设置中打开",
      showCancel: false,
      success() {
        //没有开对应的权限，打开app的系统权限管理页
        let Intent = plus.android.importClass("android.content.Intent");
        let Settings = plus.android.importClass("android.provider.Settings");
        let Uri = plus.android.importClass("android.net.Uri");
        let mainActivity = plus.android.runtimeMainActivity();
        let intent = new Intent();
        intent.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
        let uri = Uri.fromParts("package", mainActivity.getPackageName(), null);
        intent.setData(uri);
        mainActivity.startActivity(intent);
      },
    });
  } else {
    // 有权限
  }
});
```
