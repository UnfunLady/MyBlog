---
title: "uniapp H5跨域配置备忘录"
catalog: true
date: 2023-04-28 11:37:19
subtitle: "省的忘了"
header-img: 
tags:
- uniapp
- JavaScript
catagories:
- uniapp
---
### 笔记描述
> 用于解决浏览器调试时候跨域问题
### 主要代码
```json
    "h5" : {
        "template" : "h5.html",
        "router" : {
            "mode" : "hash",
            "base" : "/h5"
        },
        "optimization" : {
            "treeShaking" : {
                "enable" : true
            }
        },
        "title" : "xxx",
        "domain" : "/app",
        "devServer" : {
            "disableHostCheck" : true,
            "proxy" : {
                "/api" : {
                    "target" : "https://xxx/api",
                    "changeOrigin" : true,
                    "secure" : false,
                    "pathRewrite" : {
                        "^/api" : ""
                    }
                },
                "/yyy" : {
                    "target" : "https://xxx/api",
                    "changeOrigin" : true,
                    "secure" : false,
                    "pathRewrite" : {
                        "^/yyy" : ""
                    }
                },
                "/gjy" : {
                    "target" :"https://xxx/api",
                    "changeOrigin" : true,
                    "secure" : false,
                    "pathRewrite" : {
                        "^/gjy" : ""
                    }
                },
                "/uapi" : {
                    "target" : "https://xxx/api",
                    "changeOrigin" : true,
                    "secure" : false,
                    "pathRewrite" : {
                        "^/uapi" : ""
                    }
                }
            },
            "https" : false
        }
```