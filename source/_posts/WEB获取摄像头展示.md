---
title: "获取摄像头展示到视频流"
catalog: true
date: 2025-01-06 18:59:13
subtitle: "webrtc视频流展示"
header-img:
tags:
  - webrtc
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 获取设备的摄像头展示

### 主要代码

```js
// 检查媒体设备支持
if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
  message.warn("您的浏览器不支持视频功能");
  return;
}

// 请求媒体权限以检查是否有访问权限
try {
  const stream = await navigator.mediaDevices.getUserMedia({
    video: true,
    audio: true,
  });
  // 如果成功获取，立即停止流
  stream.getTracks().forEach((track) => track.stop());
} catch (error) {
  // 没有权限或其他错误
  message.warn("请在提示时授予摄像头和麦克风权限");
}

// 开启方法
// 请求媒体流
mediaStream = await navigator.mediaDevices.getUserMedia({ video: true });
videoElement.style.transform = "scaleX(-1)"; // 镜像效果
videoElement.style.transform = "rotateY(180deg)"; // 可选，根据需要旋转180度以达到完全镜像效果
// 将流连接到视频元素
videoElement.srcObject = mediaStream;
// 等待视频元数据加载完成
await new Promise((resolve) => {
  videoElement.onloadedmetadata = () => resolve();
});
// 播放视频
await videoElement.play();

// 停止视频流
function stopVideoStream() {
  if (!mediaStream) return;
  // 停止所有轨道
  mediaStream.getTracks().forEach((track) => track.stop());
  mediaStream = null;
  // 重置视频元素
  videoElement.srcObject = null;
  message.info("摄像头已关闭");
}
```
