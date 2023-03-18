---
title: "vue3根据用户名生成水印"
catalog: true
date: 2022-09-08 13:36:48
subtitle: "学习经验"
header-img: 
tags:
- Vue3
catagories:
- Vue
---
### 笔记描述
> 根据提供的用户名和用户账号 生成水印在项目最底层 表明身份
### 详细代码
```bash
// 生成背景
  watch(user, (newValue, oldValue) => {
    // 如果有用户名
    if (user.userInfo.userList.userInfo['nickname'] != undefined) {
      // 根据用户名字和账号绘制背景图
      // 作者：https://www.cnblogs.com/lulu-beibei/p/15918996.html
      var drawAndShareImage = function (text: string, text1: string, callback: Function) {
        var canvas = document.createElement('canvas')
        canvas.width = 570
        canvas.height = 200
        var context = canvas.getContext('2d')
        context.rect(0, 0, canvas.width, canvas.height)
        var h = null
        var w = null
        for (let i = 0; i < 2; i++) {
          if (i === 0) {
            w = 0
            h = 70
          } else if (i === 1) {
            w = 250
            h = 120
          }
          context.rotate((-8 * Math.PI) / 180) // 水印初始偏转角度
          context.font = '14px microsoft yahei'
          context.fillStyle = 'rgba(0, 0, 0, .15)'
          var mainText = text + '(' + text1 + ')'
          context.fillText(mainText, w, h)
          context.rotate((8 * Math.PI) / 180) // 把水印偏转角度调整为原来的，不然他会一直转
        }
        callback(canvas.toDataURL('image/png'))
      }
      var div1 = document.createElement('div')
      div1.className = 'needNameDw'
      document.getElementById('app').appendChild(div1)
      const img = document.getElementsByClassName('needNameDw')[0]
      drawAndShareImage(user.userInfo.userList.userInfo['nickname'], user.userInfo.userList.userInfo['username'], (url: string) => {
        // 需要覆盖所有dom 加上z-index: 9999;
        img.setAttribute('style', 'background:url("' + url + '");position: absolute;top: 0;left: 0;width: 100%;height: 100%;pointer-events: none;')
      })
    }
  }, { immediate: true, deep: true })
```
---