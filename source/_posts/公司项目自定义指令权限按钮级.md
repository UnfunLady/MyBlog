---
title: "公司项目自定义指令权限按钮级"
catalog: true
date: 2022-12-31 13:23:12
subtitle: "学习经验."
header-img: 
tags:
- Vue
catagories:
- Vue
---

### 笔记描述
> 公司里的前辈写的自定义指令 代码 拷贝来参考一下
### 详细代码
 ```JS
 // 页面内按钮过滤
import store from '@/store/index.js';

export default {
  inserted: function (el, binding, vnode) {
    // 获取当前路由name
    // 如果页面为同一模块下的子页面则取最上级权限
    let routeName = vnode.context.$route.meta.group
      ? vnode.context.$route.meta.group
      : vnode.context.$route.name;
    // 超级管理员
    if (store.state.user.userLoginInfo.isSuperMan) {
      return true;
    }
    // 获取功能点权限
    let functionList = store.state.user.privilegeFunctionPointsMap.get(routeName);
    // 有权限
    if (functionList && functionList.includes(binding.value)) {
      return true;
    } else {
  // 取消el
      el.parentNode.removeChild(el);
    }
  }
};
 ```

---



