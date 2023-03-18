---
title: "Vue3 Pinia基本用法"
catalog: true
date: 2022-09-11 13:23:12
subtitle: "学习经验."
header-img: 
tags:
- Vue3
catagories:
- Vue
---

### 笔记描述
> 代替Vuex管理仓库的插件 Pinia 可以模块化很方便
### 详细代码
 ```
// ----app.vue-----
// import { createPinia } from 'pinia'
// import { usePersist } from 'pinia-use-persist'
// pinia.use(usePersist);
// app.use(pinia).xxxxxxx

import { defineStore } from "pinia";
interface userInfo {
  isLogin: boolean,
  userToken: string | number,
  userInfo: []
}
class userInit {
  userList: userInfo = {
    isLogin: false,
    userToken: localStorage.getItem('userToken') || '',
    userInfo: []
  }
}
// 导出user
export default defineStore("user", {
  state: () => {
    return {
      userInfo: new userInit()
    }
  },
  actions: {
    setToken(token: string) {
      this.userInfo.userList.userToken = token;
      // localStorage.setItem('userToken', token);
      this.userInfo.userList.isLogin = true;
    },
    // 设置用户信息
    setUserInfo(userInfo: []) {
      this.userInfo.userList.userInfo = userInfo;
      // localStorage.setItem('userLoginInfo', JSON.stringify(userInfo))
    },
    // 退出登录
    userOut() {
      this.userInfo.userList = {
        isLogin: false,
        userToken: '',
        userInfo: []
      }
    }
  },
  getters: {
    // 获取token
    getUserToken(state) {
      return state.userInfo.userList.userToken
    }
  },
  persist: {
    enabled: true,
  }
})

 ```

---



