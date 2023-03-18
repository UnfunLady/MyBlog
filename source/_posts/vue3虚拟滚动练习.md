---
title: "vue3虚拟滚动练习"
catalog: true
date: 2022-09-03 10:22:12
subtitle: "学习经验"
header-img: 
tags:
- Vue3
catagories:
- vue3
---

### 笔记描述
> 一般用于多数据 如多图片加载使用

### 详细代码
```
<template>
  <div class="viewport" ref="viewport" :style="{ '--itemHeight': props.itemHeight + 'px' }"
  @scroll="debounce(onScroll, 600)()">
  <div class="scrollbar" ref="scrollbar">
  <div class="list" ref="list">
  <div class="listItem" v-for="(item, index) in list" :key="index">
  {{ item.n }}
</div>
</div>
</div>
</div>
</template>


<script  setup>
  import { onMounted, getCurrentInstance, defineProps, computed } from 'vue'
  const props = defineProps({
    // 目标的高度
    itemHeight: {
      type: Number || String,
      default: 40
    },
    // 要显示多少条
    viewCount: {
      type: Number || String,
      default: 20
    },
    // 默认开始位置
    start: {
      type: Number || String,
      default: 0
    },
    // 结束位置
    end: {
      type: Number || String,
      default: 20
    },
    // 传入的数据
    bigData: {
      type: Array,
      default: []
    }
  });
  const start = ref(props.start);
  const end = ref(props.end);
  const current = getCurrentInstance()
  // const bigData = new Array(2000).fill(null).map((item, index) => { return { n: index + 1 } })
  const list = computed({
    get() {
      // 获取开头到结束的数量 显示
      return props.bigData.slice(start.value, end.value)
    },
    set() {
      return props.bigData.slice(start.value, end.value)
    }
  })
  onMounted(() => {
    // 初始化展示的总数据的滚动高度 
    current.refs.scrollbar.style.height = props.itemHeight * props.bigData.length + 'px';
    // 初始化可视区域高度
    current.refs.viewport.style.height = props.itemHeight * props.viewCount + 'px';
  })
  const onScroll = () => {
    // 获取滚动的距离
    const offset = current.refs.viewport.scrollTop;
    // 开始位置=滚动的距离/目标的高度 取整 获得滚动的个数
    start.value = Math.round(offset / props.itemHeight);
    // 结束的位置=开始位置+展示的个术
    end.value = start.value + props.viewCount;
    // 滚动时将盒子移动回去 看上去就像没滚动一样
    current.refs.list.style.transform = `translateY(${offset}px)`
  }

  // 防抖
  const debounce = function (fn, wait) {
    // 自由变量，debounce执行完成被释放，time也不会被释放
    let time;
    // 返回一个闭包
    return function () {
      // 清除上一次的定时器
      if (time) {
        clearTimeout(time);
      };
      // wait时间后执行
      time = setTimeout(fn, wait);


    }
  };



</script>








<style lang='scss' scoped>
  .viewport {
    width: 100%;
    // height: 400px;
    position: fixed;
    top: 100px;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    overflow-y: scroll;
  }


  .scrollbar {
    // height: 3000px;
  }


  .list {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;


    .listItem {
      // height: 20px;
      border-top: 1px solid #b9b9b9a6;
      line-height: var(--itemHeight);
      box-sizing: border-box;
      height: var(--itemHeight);
    }
  }
</style>
```