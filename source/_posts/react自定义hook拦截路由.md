---
title: "react自定义hook拦截路由"
catalog: true
date: 2022-09-05 17:28:18
subtitle: "写项目时候遇到的"
header-img: 
tags:
- React
catagories:
- React
---
### 笔记描述
> 写项目的时候 判断用户是否登录用的
### 详细代码
```jsx
import { useEffect, useState } from "react"
import { Outlet, useLocation, useNavigate } from "react-router-dom"
import { useSelector } from 'react-redux'
import routes from "../../router"
import eroutes from "../../router/employeRouter"
import { message } from "antd"
// 筛选路由 如果匹配了路径就返回路由信息 没有就返回null
// 不能用map 之类 不能return终止的
const checkAuth = (routers: any, path: String) => {
  for (const route of routers) {
    // 传入的路由遍历 判断是否有absolutePath 有就返回没就默认path
    if ((route.absolutePath ? route.absolutePath : route.path) === path) {
      return route
    } else {
      // 如果有children属性则继续递归
      if (route.children) {
        const res: any = checkAuth(route.children, path)
        return res
      }
    }
  }
  return null
}
// 筛选匹配的路由 返回信息出去
const checkRouterAuth = (path: String, level: number = 1) => {
  let auth = null
  if (level === 1) {
    auth = checkAuth(routes, path)
  } else {
    auth = checkAuth(eroutes, path)
  }
  return auth
}
// 路由鉴权
const RouterBeforeEach = () => {
  const navigate = useNavigate()
  const location = useLocation()
  const [auth, setAuth] = useState(false)
  // 从store获取登录状态
  const isLogin = useSelector((state: any) => {
    return state.user.userList.isLogin
  })
  const userToken = useSelector((state: any) => {
    return state.user.userList.userToken
  })
  const userInfo = useSelector((state: any) => {
    return state.user.userList.userInfo
  })
  useEffect(() => {
    // 获取和当前路由路径匹配的路由
    if (userInfo.level == 1) {
      const obj: any = checkRouterAuth(location.pathname, 1)
      // 登录鉴权
      if (obj && obj.auth && isLogin == false && userToken == '') {
        setAuth(false)
        navigate('/loginView', { replace: true })
        message.warning('请先登录！')
      } else {
        // 鉴权成功返回true
        setAuth(true)
      }
    } else {
      const obj: any = checkRouterAuth(location.pathname, 2)
      // 登录鉴权
      if (obj && obj.auth && isLogin == false && userToken == '') {
        setAuth(false)
        navigate('/loginView', { replace: true })
        message.warning('请先登录！')
      } else {
        // 鉴权成功返回true
        setAuth(true)
      }
    }


  }, [location.pathname])
  // 返回outlet或null
  return auth ? <Outlet /> : null
}


export default RouterBeforeEach
```