
---
title: 前端框架开发文档
date: 2020-08-27 10:51:45
---

#### 目录结构

    
    |-- vue.config.js // 配置文件
    |-- build //打包
    |-- public //公共资源 目前有echart，day.min 优化
    |-- server //公共服务方法， 包含log，websocket，mock等
    |-- src
    |   |-- App.vue
    |   |-- main.js //入口
    |   |-- permission.js //权限
    |   |-- settings.js
    |   |-- api //请求接口
    |   |   |-- biz.js
    |   |-- assets //资源
    |   |-- biz //业务类组件
    |   |-- components //功能组件
    |   |-- directives //自定义指令
    |   |-- icons
    |   |-- layout //布局
    |   |-- mixins
    |   |-- router//路由
    |   |-- store //状态管理
    |   |   |-- getters.js
    |   |   |-- index.js //新增模块需在index中构造
    |   |   |-- modules //模块
    |   |       |-- biz.js
    |   |-- styles //样式
    |   |-- track
    |   |-- utils //工具，公共方法包含storage，validate等，请务必熟悉
    |   |-- views //页面
    |       |-- board 
    |       |-- dss //项目目录 名字可自定义，其余模块为框架自有文件不可删减
    |       |-- error
    |       |-- help
    |       |-- login
    |       |-- profile
    |       |-- redirect
    |       |   |-- index.vue
    |       |-- system

### 路由配置-本项目中路由配置整体遵循vue-router配置规则

1. children 中的path  
  - `'/xxx_router'`  顶级路由 路由显示格式为  `project_url/xxx_router`
  - `'xxx'` 层级路由   `project_url/parent_router/xxx_router` 以此类推
  
2. 当子路由长度仅为1时 将直接显示子路由title 不再做分级显示


```bash
   { 
      path: '/xx',//路径
      component: Layout,
      redirect: '/test',
      meta: { title: '测试路由父类', icon: 'dashboard', affix: true },
      children: [//子路由
         {
            path: 'test',
            name: 'test',
            hidden: true,//true/false 隐藏/显示 无hidden字段默认显示
            component: () => import('@/views/dss/pages/test'),
            meta: { title: '测试路由', icon: 'dashboard', affix: true }
         },
      ]
    }
```
### GET，POST请求传参

```bash
//post的请求参数格式为data:params
import { base_fof, base_hdr} from './base.js'
export function updateTree(params) {
  return request({
    url: base_fof+'/poc/subjectTree/v1/update',
    method: 'post',
    data: params
  })
}
//get的请求参数格式为params:params，可简写为params
export function getCopyTree(params) {
  return request({
    url: base_fof+'/poc/modelCopy/v1/getTree',
    method: 'get',
    params
  })
}
```


### 页面中如何调用对应模块API
```bash
//先导入对应路径下的api
import { captcha } from '@/api/login'

//调用
captcha(param).then(response => {
   const { data } = response
}).catch(e=>{
   
})
    

```

### 开发规范注释之方法
```bash
 /**
 * @desc 函数节流 //功能
 * @param func 函数 //参数名 参数描述
 * @param wait 延迟执行毫秒数
 * @param type 1 表时间戳版，2 表定时器版
 */
```

### 开发规范注释之行内
```bash
 if{//注释

 }

 //注释
 if{

 }
```

### views-xxx（dss）命名规划

- 文件：功能组件为大驼峰  TestName
- 页面： test-name
- 变量/方法：小驼峰 testName
- class test-name
未提到部份参考https://cn.vuejs.org/v2/style-guide/index.html
