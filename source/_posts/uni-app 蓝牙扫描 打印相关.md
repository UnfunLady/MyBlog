---
title: "uni-app 蓝牙连接设备打印小票"
catalog: true
date: 2023-05-08 22:12:11
subtitle: "用户项目中连接客户打印机打印小票"
header-img:
tags:
  - uni-app
  - 蓝牙
  - 打印
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> 用于解决 uniapp 安卓蓝牙打印问题
> 这一块相当复杂 当时搞了很久也很容易忘记 备忘一下 后续对着源码好看点

### 主要代码

```js
// 原生插件
const jcapi = uni.requireNativePlugin("JCSDK-JCApiModule");
// onload调用初始化SDK;
let _this = this;
jcapi.initSDK();
// 监听页码回调
jcapi.didReadPrintCountInfo(function (r) {
  console.log(r);
});
// 监听错误回调
jcapi.didReadPrintErrorInfo(function (r) {
  console.log(r);
  if (r.code == 23) {
    // 打印机断开连接

    // 设备列表清空
    _this.serverList = [];
    // 连接的id
    _this.deviceId = "";
    // 重新搜索
    _this.$emit("getDevices", true);
  }
  uni.showToast({
    icon: "none",
    title: JSON.stringify(r),
    duration: 2 * 1000,
  });
});

// beforedestory停止搜索和中断
jcapi.stopBluetoothDevicesDiscovery();
jcapi.close();

// mounted初始化调用打开蓝牙适配器
var _this = this;
uni.openBluetoothAdapter({
  complete(e) {
    console.log(e);
    if (!e.errCode) {
      // 获取是否有历史连接id
      let blueDevices = uni.getStorageSync("blueDevices");
      if (blueDevices && blueDevices.deviceId) {
        // 开始搜索方法
        _this.discoveryPrinter();
      }
      console.log("初始化完成");
    } else if (e.errCode == 10001) {
      uni.showToast({
        icon: "none",
        title: "请打开手机蓝牙",
      });
    } else {
      uni.showToast({
        icon: "none",
        title: e.errMsg,
      });
    }
  },
});

// discoverPrinter方法
let _this = this;
// 获取历史连接设备
let blueDevices = uni.getStorageSync("blueDevices");
uni.openBluetoothAdapter({
  // 确认蓝牙是否打开
  success(r) {
    uni.showLoading({
      title: "搜索中 可能需要几秒钟..",
      mask: true,
    });
    // 未授予蓝牙相关权限和未打开手机定位会搜索不到设备
    jcapi.getBluetoothDevices(function (r) {
      console.log("device:" + JSON.stringify(r));
      // 搜索到设备回调
      uni.hideLoading();
      _this.devices = [];
      _this.deviceIds = [];
      r.map((i) => {
        if (_this.deviceIds.indexOf(i.address) == -1) {
          const item = {
            name: i.name,
            deviceId: i.address,
            index: 0,
          };
          // 加入有历史连接的id直接连接
          if (blueDevices && blueDevices.deviceId == i.address) {
            item.index = 1;
            uni.showLoading({
              title: "连接中",
              mask: true,
            });
            // 连接方法
            _this.connect(i.address);
          }
          // 如果没有历史连接就在数组加入设备展示到页面上用于用户手动点击连接
          _this.devices.push(item);
          _this.deviceIds.push(i.address);
        }
      });
    });
  },
  fail(e) {
    uni.showModal({
      confirmText: "打开蓝牙失败",
    });
    console.log("开启蓝牙设备失败" + e);
  },
});

// connect方法 传入设备deviceId
var _this = this;
try {
  uni.createBLEConnection({
    deviceId,
    complete(e) {
      console.log(e, _this.deviceId);
      if (!e.errCode) {
        //获取蓝牙设备所有服务(service)。
        setTimeout(() => {
          _this.getBLEDeviceServices(deviceId);
        }, 2000);
      } else {
        uni.hideLoading();
        jcapi.stopBluetoothDevicesDiscovery();
        jcapi.close();
        _this.$emit("devices", true);
        if (e.errCode == -1) {
          //获取蓝牙设备所有服务(service)。
          _this.getBLEDeviceServices(deviceId);
          uni.showToast({
            icon: "none",
            title: "设备已连接",
          });
        } else {
          uni.showToast({
            icon: "none",
            title: "连接设备失败",
          });
        }
      }
    },
  });
} catch (e) {
  jcapi.stopBluetoothDevicesDiscovery();
  jcapi.close();
  uni.hideLoading();
  uni.showToast({
    icon: "none",
    title: "连接设备失败",
  });
  //TODO handle the exception
}

// getBLEDeviceServices方法
var _this = this;
try {
  uni.getBLEDeviceServices({
    // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接
    deviceId,
    success: (res) => {
      console.log("device services:", res);
      if (res.services && res.services.length > 0) {
        res.services.map((item) => {
          uni.getBLEDeviceCharacteristics({
            // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立链接
            deviceId,
            // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
            serviceId: item.uuid,
            success: (res2) => {
              if (res2.characteristics && res2.characteristics.length > 0) {
                res2.characteristics.map((ite) => {
                  if (ite.properties.write) {
                    if (_this.serverList.length == 0) {
                      _this.deviceId = deviceId;
                      _this.serverList.push({
                        serviceId: item.uuid,
                        characteristicId: ite.uuid,
                      });
                      _this.$emit("getDevices", false);
                      jcapi.stopBluetoothDevicesDiscovery();
                      // 连接成功了 后续可以打印了
                      _this.$u.toast("连接成功");
                      uni.hideLoading();
                    }
                  }
                });
              }
            },
            fail: (err) => {
              console.log(err);
              jcapi.stopBluetoothDevicesDiscovery();
              jcapi.close();
              uni.hideLoading();
              _this.$u.toast(err);
            },
          });
        });
      } else {
        _this.$emit("getDevices", true);
        jcapi.stopBluetoothDevicesDiscovery();
        jcapi.close();
        uni.hideLoading();
      }
    },
    fail: (err) => {
      console.log(err);
      uni.hideLoading();
      _this.$u.toast(err);
      _this.$emit("getDevices", true);
      jcapi.stopBluetoothDevicesDiscovery();
      jcapi.close();
    },
  });
} catch (e) {
  console.log(e);
  uni.hideLoading();
  //TODO handle the exception
}

// 开始的打印方法

let printerJobs = new PrinterJobs();
printerJobs
  .lineFeed()
  .setAlign("ct")
  .setSize(1, 1)
  .setBold(true)
  .print(`xxx有限公司xxx业务`)
  .print(printerUtil.fillLine(" "))
  .setAlign("lt")
  .setSize(1, 1)
  .setBold(false)
  .print(`xxx：${_this.pageData.occTime || " "}`)
  .print(`xx${_this.pageData.dispatchNo || " "}`)
  .print(`xx${_this.pageData.rescueNo || " "}`);

let buffer = printerJobs.buffer();
_this.printbuffs(buffer, "first");
async printbuffs(buffer, tag) {
      // 1.并行调用多次会存在写失败的可能性
      // 2.建议每次写入不超过20字节
      // 分包处理，延时调用
      const maxChunk = 20;
      for (let i = 0, j = 0, length = buffer.byteLength; i < length; i += maxChunk, j++) {
          let subPackage = buffer.slice(i, i + maxChunk <= length ? i + maxChunk : length);
          let flag = i + maxChunk >= length ? true : false;
          await this.printbuff(subPackage, flag, tag);
      }
  }

// printbuff方法
var _this = this;
return new Promise((resolve) => {
  uni.writeBLECharacteristicValue({
    // 这里的 deviceId 需要在 getBluetoothDevices 或 onBluetoothDeviceFound 接口中获取
    deviceId: _this.deviceId,
    // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
    serviceId: _this.serverList[0].serviceId,
    // 这里的 characteristicId 需要在 getBLEDeviceCharacteristics 接口中获取
    characteristicId: _this.serverList[0].characteristicId,
    // 这里的value是ArrayBuffer类型
    value: buffer,
    success: (res) => {
      setTimeout(() => {
        resolve(1);
        if (flag && tag == "first") {
          _this.printSign();
        }
        if (flag && tag == "second") {
          if (_this.pageData.qrCodeUrl) {
            _this.printQrCode();
          } else {
            _this.lastPrint();
          }
        }
        if (flag && tag == "three") {
          if (_this.pageData.qrCodeUrl) {
            _this.printQrCode();
          } else {
            _this.lastPrint();
          }
        }
        if (flag && tag == "four") {
          _this.lastPrint();
        }
        if (flag && tag == "finish") {
          uni.hideLoading();
          _this.$u.toast("打印成功");
          jcapi.stopBluetoothDevicesDiscovery();
          jcapi.close();
          _this.$emit("close", true);
          this.$u.vuex("printing", false);
          let blueDevices = uni.getStorageSync("blueDevices");
          if (blueDevices) {
            if (blueDevices != _this.deviceId) {
              uni.setStorageSync("blueDevices", {
                deviceId: _this.deviceId,
              });
            }
          } else {
            uni.setStorageSync("blueDevices", {
              deviceId: _this.deviceId,
            });
          }
        }
      }, 60);
    },
    fail: (res) => {
      uni.hideLoading();
      this.$u.vuex("printing", false);
      _this.$u.toast("打印失败");
      jcapi.stopBluetoothDevicesDiscovery();
      jcapi.close();
      _this.$emit("close", true);
      console.log("writeBLECharacteristicValue fail", res.errMsg);
    },
  });
});

```

## 部分截图

<div style="display:flex;width:100%;flex-wrap:wrap">
<img src="../uni-app 蓝牙扫描 打印相关/print1.jpg" width="300" />
<img src="../uni-app 蓝牙扫描 打印相关/print2.jpg"  width="300"  />
</div>
