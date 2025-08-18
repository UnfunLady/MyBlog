---
title: "uni-app 终止请求  播放音乐震动"
catalog: true
date: 2023-05-26 14:12:15
subtitle: "终止请求 音乐震动"
header-img:
tags:
  - uni-app
  - JavaScript
catagories:
  - uni-app
---

### 笔记描述

> 很久没更新 比较忙 用于解决部分操作调用大量接口 返回或者前进后需要控制终止请求 已经 websocket 监听到消息后续提示和震动
### 主要代码

```js
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
const store = new Vuex.Store({
  state: {
    requestTaskList: [],
  },
  mutations: {
    addTask(state, data) {
      state.requestTaskList.push(data);
    },
    //移除记录
    removeTask(state, data) {
      state.requestTaskList = state.requestTaskList.filter(
        (item) => item.uniqueId != data.uniqueId
      );
    },
    //终止请求记录
    clearList(state, data) {
      state.requestTaskList.forEach((item) => item.abort());
      state.requestTaskList = [];
    },
  },
});
// 播放音樂
if (!state.innerAudioContext) {
  try {
    state.audioStart = true;
    const innerAudioContext = uni.createInnerAudioContext();
    innerAudioContext.autoplay = true;
    innerAudioContext.loop = false;
    innerAudioContext.volume = 1;
    innerAudioContext.src = "/static/music/notice.mp3"; //铃声文件的路径
    innerAudioContext.onPlay(() => {
      //console.log('开始播放');
    });
    innerAudioContext.onError((res) => {
      state.audioStart = false;
    });
    innerAudioContext.onEnded((res) => {
      state.audioStart = false;
    });
    state.innerAudioContext = innerAudioContext;
    // 手機震動
    uni.vibrateLong({
      success: function () {
        state.audioStart = false;
        //console.log('shak success');
      },
    });
  } catch (e) {
    if (state.innerAudioContext) {
      state.innerAudioContext.stop();
      state.innerAudioContext.destroy();
      state.innerAudioContext = null;
      state.audioStart = false;
    }
  }
} else {
  state.innerAudioContext.play();
  uni.vibrateLong({
    success: function () {
      state.audioStart = false;
      //console.log('shak success');
    },
  });
}
```
