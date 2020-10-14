---
title: Vue开发技巧
date: 2020-07-30 10:38:19
tags:
---
## 小型状态管理器
---
大型项目中的数据状态会比较复杂，一般使用vuex来管理，那在一些小型项目中如何自己解决

在vue2.6.0+版本中，新增的vue.observable可以帮我们解决这个问题，他能让一个对象变成响应式数据

vue.observable让一个对象可响应。vue内部会用它来处理data函数返回的对象,返回的对象可以直接用于`渲染函数`和`计算属性`内，


```bash
// store.js
import Vue from 'vue'
export const state = Vue.observable({
  count: 0 
})
```
使用：
```bash
<div @click="setCount">{{ count }}</div>
import {state} from '../store.js'
export default {
    computed: {
      count() {
        return state.count
      }
    },
    methods: {
      setCount() {
        state.count++
      }
    }
}
```
当然你也可以自定义 mutation 来复用更改状态的方法：
```bash
import Vue from 'vue'
export const state = Vue.observable({
  count: 0 
})
export const mutations = {
  SET_COUNT(payload) {
    if (payload > 0) {
      state.count = payload
    }
  }
}
```
使用：
```bash
import {state, mutations} from '../store.js'

export default {
  computed: {
    count() {
        return state.count
    }
  },
  methods: {
    setCount() {
        mutations.SET_COUNT(100)
    }
  }
}
```
参考文档：https://cn.vuejs.org/v2/api/#Vue-observable
https://mp.weixin.qq.com/s/-e6VZGfSu9PFiGCCI56YFw


## 小型状态管理器
---
通常定义叔数据观察，会使用选项的方式在watch中配置
```bash
export default {
    data() {
        return {
            count: 1      
        }
    },
    watch: {
        count(newVal) {
            console.log('count 新值：'+newVal)
        }
    }
}
```
除此之外，数据观察还有另一种函数式定义的方式：
```bash
export default {
    data() {
        return {
            count: 1      
        }
    },
    created() {
        this.$watch('count', function(){
            console.log('count 新值：'+newVal)
        })
    }
}
```
它和前者的作用一样，但这种方式使定义数据观察更灵活，而且 $watch 会返回一个取消观察函数，用来停止触发回调：
```bash
let unwatchFn = this.$watch('count', function(){
    console.log('count 新值：'+newVal)
})
this.count = 2 // log: count 新值：2
unwatchFn()
this.count = 3 // 什么都没有发生...
```
$watch 第三个参数接收一个配置选项：
```bash
this.$watch('count', function(){
    console.log('count 新值：'+newVal)
}, {
    immediate: true // 立即执行watch
})
```
参考文档：cn.vuejs.org/v2/api/#vm-watch


## 巧用template
---
相信 `v-if` 在开发中是用得最多的指令，那么你一定遇到过这样的场景，多个元素需要切换，而且切换条件都一样，一般都会使用一个元素包裹起来，在这个元素上做切换。

```bash
<div v-if="status==='ok'">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</div>
```

如果像上面的 div 只是为了切换条件而存在，还导致元素层级嵌套多一层，那么它没有“存在的意义”。
我们都知道在声明页面模板时，所有元素需要放在 `<template>` 元素内。除此之外，它还能在模板内使用，`<template>` 元素作为不可见的包裹元素，只是在运行时做处理，最终的渲染结果并不包含它。
```bash
<template>
    <div>
        <template v-if="status==='ok'">
          <h1>Title</h1>
          <p>Paragraph 1</p>
          <p>Paragraph 2</p>
        </template>
    </div>
</template>
```
同样的，我们也可以在 `<template>` 上使用 `v-for` 指令，这种方式还能解决 `v-for` 和 `v-if` 同时使用报出的警告问题。
```bash
<template v-for="item in 10">
    <div v-if="item % 2 == 0" :key="item">{{item}}</div>
</template>
```