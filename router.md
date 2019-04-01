## vue-router
前端路由，是虚拟路由，是组件的显示隐藏。

### 基本概念 router，routers，route
1. router 路由的集合体，管理路由的
2. routes 一组路由
3. route 一条路由

### 基本使用方法
```
1. 
import vueRouter from 'vue-router'
2. 
var router = new vueRouter({
  mode:'hash',  //默认hash，只有两个值，hash和history
  base:__dirname,
  routes:[]
})
3.
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})

```
### 路由的钩子事件

#### 全局钩子
```
1. router.beforeEach((to,from,next) =>{})  //to上一个路由来源，from 下一个路由，next（）下一个钩子函数
2. router.beforeResolve((to,from,next) =>{})
3. router.afterEach((to,from) => {})
```
#### 组件内的钩子
```
beforeRouteEnter (to, from, next) {
  // 在路由独享守卫后调用 不！能！获取组件实例 `this`，组件实例还没被创建
},
beforeRouteUpdate (to, from, next) {
  // 在当前路由改变，但是该组件被复用时调用 可以访问组件实例 `this`
  // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
  // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
},
beforeRouteLeave (to, from, next) {
  // 导航离开该组件的对应路由时调用，可以访问组件实例 `this`
}
```
#### 独享钩子
 ```
beforeEnter: (to, from, next) => {},
beforeLeave: (to, from, next) => {}

 ```
### 和路由关系密切的 keep-alive 缓存（还有两个钩子函数，下次继续）
#### 是Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染DOM。
```
<keep-alive>
    <router-view></router-view>
</keep-alive>
```

### 整个钩子函数的执行顺序
beforeRouteLeave:路由组件的组件离开路由前钩子，可取消路由离开。
beforeEach: 路由全局前置守卫，可用于登录验证、全局路由loading等。
beforeEnter: 路由独享守卫
beforeRouteEnter: 路由组件的组件进入路由前钩子。
beforeResolve:路由全局解析守卫
afterEach:路由全局后置钩子
beforeCreate:组件生命周期，不能访问this。
created:组件生命周期，可以访问this，不能访问dom。
beforeMount:组件生命周期
deactivated: 离开缓存组件a，或者触发a的beforeDestroy和destroyed组件销毁钩子。
mounted:访问/操作dom。
activated:进入缓存组件，进入a的嵌套子组件(如果有的话)。
执行beforeRouteEnter回调函数next。

### 组合用的标签


### demo
```
router.js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import First from '@/components/First'
import Second from '@/components/Second'
Vue.use(Router)
var router = new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/',
      component: HelloWorld,
      children:[
        {path: '/first', component: First}
      ],
      beforeEnter: (to, from, next) => {
        console.log("这里是独立钩子第一页beforeEnter")
        next()
      },
      beforeLeave: (to, from, next) => {
        console.log("这里是独立钩子第一页beforeLeave")
        next()
      }
    },
    {
      path: '/',
      component: HelloWorld,
      children:[
        {path:'/second', component:Second}
      ],
      beforeEnter: (to, from, next) => {
        console.log("这里是独立钩子第二页beforeEnter")
        next()
      },
      beforeLeave: (to, from, next) => {
        console.log("这里是独立钩子第二页beforeLeave")
        next()
      }
    }
  ]
})

router.beforeEach((to,from,next) => {
  console.log('全局钩子函数beforeEach')
  next()
})

router.beforeResolve((to,from,next) => {
  console.log('全局钩子函数beforeResolve')
  next()
})

router.afterEach((to,from) => {
  console.log('全局钩子函数afterEach')
})

router.onError(callback => {
  console.log(callback,'callback')
})

export default router


first.vue
<template>
    <div>这里是第一页</div>
</template>

<script>
export default {
  name: "first",
  beforeRouteEnter (to, from, next) {
    console.log('这里是页面一beforeRouteEnter')
    next()
  },
  beforeRouteUpdate (to, from, next) {
    console.log('这里是页面一beforeRouteUpdate')
    next()
  },
  beforeRouteLeave (to, from, next) {
    console.log('这里是页面一beforeRouteLeave')
    next()
  }
}
</script>

<style scoped>

</style>

```

