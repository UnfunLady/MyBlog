---
title: "uni-app 获取应用版本相关信息 在线更新"
catalog: true
date: 2023-06-07 23:19:05
subtitle: "在线更新相关"
header-img:
tags:
  - uni-app
  - 在线更新
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> 获取应用版本相关信息弹出在线更新信息

### 主要代码

```js
plus.runtime.getProperty(plus.runtime.appid, (appInfo) => {
  // appInfo为当前应用程序的所有信息
  const version = appInfo.version ? appInfo.version : null;
  if (version) {
    // 获取最新版本比较当前版本
    _this.$u.api.order
      .getUpdateInfo()
      .then((res) => {
        if (res && res.code == 0 && res.data) {
          res.data.createTime = moment(res.data.createTime).format(
            "YYYY-MM-DD"
          );
          this.versionInfo = res.data;
          // 获取历史版本信息
          this.list = res.data.upgradeInfo.split(",");
          if (_this.compare(res.data.serverVersion, version)) {
            // 弹出更新弹窗
            this.newUpdate = true;
          } else {
            this.newUpdate = false;
          }
        }
      })
      .catch((e) => {
        console.log(e);
        this.newUpdate = false;
      });
  }
});
compare(curV, reqV) {
  if (curV && reqV) {
    //将两个版本号拆成数字
    var arr1 = curV.split('.'),
      arr2 = reqV.split('.');
    var minLength = Math.min(arr1.length, arr2.length),
      position = 0,
      diff = 0;
    //依次比较版本号每一位大小，当对比得出结果后跳出循环（后文有简单介绍）
    while (position < minLength && (diff = parseInt(arr1[position]) - parseInt(arr2[position])) == 0) {
      position++;
    }
    diff = diff != 0 ? diff : arr1.length - arr2.length;
    //若curV大于reqV，则返回true
    return diff > 0;
  } else {
    //输入为空
    console.log('版本号不能为空');
    return false;
  }
}

```
## 部分截图 ##
<img src="../uni-app 获取应用版本相关信息 在线更新/p.jpg" width="300" />