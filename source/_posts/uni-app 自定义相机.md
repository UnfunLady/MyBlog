---
title: "uni-app live-pusher自定义相机"
catalog: true
date: 2023-06-21 13:29:31
subtitle: "live-pusher自定义相机"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> nvue 使用 live-pusher 自定义相机拍照

### 主要代码

```js

<view>
    <live-pusher
        id="livePusher"
        ref="livePusher"
        class="livePusher"
        mode="FHD"
        beauty="0"
        whiteness="0"
        device-position="back"
        :auto-focus="true"
        :muted="true"
        :enable-camera="true"
        :enable-mic="false"
        :zoom="true"
        :style="[{ height: cameraHeight + 'px', width: '750rpx' }]"
    ></live-pusher>
    // 增加图片元素提示
    <cover-image v-if="coverImage" class="cover-image" :style="[{ height: cameraHeight + 'px', width: '750rpx' }]" :src="coverImage" />
    <cover-view class="camera-options" :style="[{ height: optionsHeight + 'px' }]" v-if="coverImage">
        <cover-view class="camera-options-center camera-item">
            <image
                class="camera-item-image"
                src="/static/masking/snapShot.png"
                style="width: 120rpx; height: 120rpx"
                mode="scaleToFill"
                // 拍照
                @click="handleInstruct('shutter')"
            ></image>
        </cover-view>
        <cover-view class="camera-options-left camera-item">
            <image
                class="camera-item-image"
                src="/static/masking/back.png"
                style="width: 30rpx; height: 50rpx; transform: rotate(90deg)"
                mode="scaleToFill"
                // 返回
                @click="handleInstruct('back')"
            ></image>
        </cover-view>
    </cover-view>
</view>

handleInstruct(instruct) {
  switch (instruct) {
      // 返回
      case 'back':
          uni.navigateBack();
          break;
      // 快门
      case 'shutter':
          if (this.ready) {
              this.ready = false;
              this.livePusher.snapshot({
                  success: (res) => {
                      this.ready = true;
                      this.$emit('getImage', res.message.tempImagePath);
                      uni.navigateBack();
                  }
              });
          }
          break;
      // 反转
      case 'reversal':
          this.livePusher.switchCamera();
          break;
      // 相册
      case 'album':
          uni.chooseImage({
              count: 1, //默认9
              sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
              sourceType: ['album'], //从相册选择
              success: (res) => {
                  this.$emit('getImage', res.tempFilePaths[0]);
              }
          });
       break;
  }
}
```

## 部分截图

<img src="../uni-app 自定义相机/1.jpg" width="300" />
