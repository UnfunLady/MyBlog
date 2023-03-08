---
title: "React路由默认选中高光"
catalog: true
date: 2022-12-25 23:37:29
subtitle: "写项目时候遇到的"
header-img: 
tags:
- React
catagories:
- React
---
### 笔记描述
> 写项目的时候筛选路由属性带有activePath的路由用于修改默认选中路径
### 详细代码
```jsx
const checkActivePath = (routerList: Array, locationName: Array<any>) => {
  routerList.map((route: any) => {
    if (route.children) {
      // 递归循环直到当前激活的路由
      return checkActivePath(route.children, locationName)
    } else {
      // 当前激活路由判断是否有activePath
      if (route.activePath && route.activePath.length > 0) {
        if (route.path === locationName[locationName.length - 1]) {
          data.mainViewData.defaultPath = route.activePath.split('/')
          setData({ ...data })
        }
      } else {
        //   三级四级路由
        switch (locationName.length) {
          case 4:
            data.mainViewData.OpenKeys = locationName.slice(0, 3)
            break;
          case 5:
            data.mainViewData.OpenKeys = locationName.slice(0, 4)
            break;
        }
        data.mainViewData.defaultPath = locationName
        setData({ ...data })
      }
    }
  })
}

/*
       <Menu
                    onClick={onClick}
                    // 默认初始选中
                    selectedKeys={data.mainViewData.defaultPath.length > 0 ? data.mainViewData.defaultPath : location.pathname.split('/')}
                    defaultOpenKeys={data.mainViewData.OpenKeys}
                    openKeys={data.mainViewData.OpenKeys}
                    mode="inline"
                    theme={"dark"}
                    multiple={true}
                    onOpenChange={onOpenChange}
                    items={data.mainViewData.menuList}
                >
                </Menu>

*/
```