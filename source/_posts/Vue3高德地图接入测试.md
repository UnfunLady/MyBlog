---
title: "Vue3高德地图接入测试"
catalog: true
date: 2023-01-12 13:19:01
subtitle: "学习经验."
header-img: 
tags:
- Vue3
catagories:
- Vue
---

### 笔记描述
>  对选择上传的图片进行压缩 组件库则把获取到的file替换e.target.files[0]

### 详细代码
 ```vue
<template>
    <div class="container">
        <div style="background-color: #ffffff;">
            <div id="container"></div>
        </div>
    </div>
</template>
 
<script setup>
import AMapLoader from '@amap/amap-jsapi-loader';
/*在Vue3中使用时,需要引入Vue3中的shallowRef方法(使用shallowRef进行非深度监听,
因为在Vue3中所使用的Proxy拦截操作会改变JSAPI原生对象,所以此处需要区别Vue2使用方式对地图对象进行非深度监听,
否则会出现问题,建议JSAPI相关对象采用非响应式的普通对象来存储)*/
import { shallowRef } from '@vue/reactivity';
import { ref } from "vue";
// const map = shallowRef(null);
const path = ref([]);
const current_position = ref([]);
let map = null;
function initMap() {
    window._AMapSecurityConfig = {
        securityJsCode: '自己申请的密钥',
    }
    AMapLoader.load({
        "key": "", // 申请好的Web端开发者Key，首次调用 load 时必填
        "version": "2.0",   // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        "plugins": ["AMap.Scale", "AMap.ToolBar", "AMap.ControlBar", "AMap.MouseTool", "AMap.MapType", "AMap.HawkEye"],           // 需要使用的的插件列表，如比例尺'AMap.Scale'等
        // "plugins": [],           // 需要使用的的插件列表，如比例尺'AMap.Scale'等
    }).then((AMap) => {
        this.map = new AMap.Map('container', {
            viewMode: "2D",  //  是否为3D地图模式
            zoom: 13,   // 初始化地图级别
            center: [113.65553, 34.748764], //中心点坐标  郑州
            resizeEnable: true
        });
        this.mouseTool = new AMap.MouseTool(this.map);
        // 监听draw事件可获取画好的覆盖物
        this.mouseTool.on('draw', function (e) {
            this.overlays.push(e.obj);
        }.bind(this))
    }).catch(e => {
        console.log(e);
    });
}
const draw = (type) => {
    switch (type) {
        case 'marker': {
            this.mouseTool.marker({
                //同Marker的Option设置
            });
            break;
        }
        case 'polyline': {
            this.mouseTool.polyline({
                strokeColor: '#80d8ff'
                //同Polyline的Option设置
            });
            break;
        }
        case 'polygon': {
            this.mouseTool.polygon({
                fillColor: '#00b0ff',
                strokeColor: '#80d8ff'
                //同Polygon的Option设置
            });
            break;
        }
        case 'rectangle': {
            this.mouseTool.rectangle({
                fillColor: '#00b0ff',
                strokeColor: '#80d8ff'
                //同Polygon的Option设置
            });
            break;
        }
        case 'circle': {
            this.mouseTool.circle({
                fillColor: '#00b0ff',
                strokeColor: '#80d8ff'
                //同Circle的Option设置
            });
            break;
        }
    }

}

initMap()
</script>
 
<style>
#container {
    padding: 0px;
    margin: 0px;
    width: 100%;
    height: 800px;
}

/* 隐藏高德logo  */
.amap-logo {
    display: none !important;
}

/* 隐藏高德版权  */
.amap-copyright {
    display: none !important;
}
</style>
 ```





