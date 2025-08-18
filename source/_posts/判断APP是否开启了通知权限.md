---
title: "判断APP是否开启了通知权限"
catalog: true
date: 2024-11-23 00:48:26
subtitle: "通知权限校验"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 APP 端判断是否开启了通知权限以免收不到 push 消息

### 主要代码

```js
// 判断通知权限是否开启
function isOn() {
  // #ifdef APP-PLUS
  if (plus.os.name == "Android") {
    var main = plus.android.runtimeMainActivity();
    var NotificationManagerCompat = plus.android.importClass(
      "android.support.v4.app.NotificationManagerCompat"
    );
    if (NotificationManagerCompat == null) {
      NotificationManagerCompat = plus.android.importClass(
        "androidx.core.app.NotificationManagerCompat"
      );
    }
    var areNotificationsEnabled =
      NotificationManagerCompat.from(main).areNotificationsEnabled();
    return areNotificationsEnabled;
  } else if (plus.os.name == "iOS") {
    var isIosOn = undefined;
    var types = 0;
    var app = plus.ios.invoke("UIApplication", "sharedApplication");
    var settings = plus.ios.invoke(app, "currentUserNotificationSettings");
    if (settings) {
      types = settings.plusGetAttribute("types");
      plus.ios.deleteObject(settings);
    } else {
      types = plus.ios.invoke(app, "enabledRemoteNotificationTypes");
    }
    plus.ios.deleteObject(app);
    isIosOn = 0 != types;
    return isIosOn;
  }
  // #endif
}

// 前往系统设置页面开启或关闭通知权限
function switchPushPermissions() {
  // #ifdef APP-PLUS
  let title = isOn() ? "通知权限关闭提醒" : "通知权限开启提醒";
  let content = isOn()
    ? "通知权限已开启，是否前往设置关闭？"
    : "您还没有开启通知权限，无法接受到消息通知，是否前往设置？";
  // 打开安卓系统设置页面
  let openAndroidSetting = () => {
    var main = plus.android.runtimeMainActivity();
    var pkName = main.getPackageName();
    var uid = main.getApplicationInfo().plusGetAttribute("uid");
    var Intent = plus.android.importClass("android.content.Intent");
    var Build = plus.android.importClass("android.os.Build");
    //android 8.0引导
    if (Build.VERSION.SDK_INT >= 26) {
      var intent = new Intent("android.settings.APP_NOTIFICATION_SETTINGS");
      intent.putExtra("android.provider.extra.APP_PACKAGE", pkName);
    } else if (Build.VERSION.SDK_INT >= 21) {
      //android 5.0-7.0
      var intent = new Intent("android.settings.APP_NOTIFICATION_SETTINGS");
      intent.putExtra("app_package", pkName);
      intent.putExtra("app_uid", uid);
    } else {
      //(<21)其他--跳转到该应用管理的详情页
      intent.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
      var uri = Uri.fromParts("package", mainActivity.getPackageName(), null);
      intent.setData(uri);
    }
    // 跳转到该应用的系统通知设置页
    main.startActivity(intent);
  };
  // 打开苹果系统设置页面
  let openIOSSetting = () => {
    var app = plus.ios.invoke("UIApplication", "sharedApplication");
    var setting = plus.ios.invoke("NSURL", "URLWithString:", "app-settings:");
    plus.ios.invoke(app, "openURL:", setting);
    plus.ios.deleteObject(setting);
    plus.ios.deleteObject(app);
  };
  // 弹窗提醒开通或关闭权限，点击确认后，跳转到系统设置页面进行设置
  uni.showModal({
    title: title,
    content: content,
    showCancel: true,
    confirmColor: "#ff903f",
    success: (res) => {
      if (res.confirm) {
        if (plus.os.name == "Android") {
          openAndroidSetting();
        } else if (plus.os.name == "iOS") {
          openIOSSetting();
        }
      }
    },
  });
  // #endif
}

export default {
  isOn,
  switchPushPermissions,
};

// #ifdef APP
try {
  const flag = notice.isOn();
  if (!flag) {
    notice.switchPushPermissions();
  }
} catch (e) {
  console.log(e, notice);
  //TODO handle the exception
}
// #endif
```
