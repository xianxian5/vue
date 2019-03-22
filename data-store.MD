# VUE数据通信
### 1.sessionStorage（h5的存储方式）
1. setItem存储value 
```
sessionStorage.setItem("key", "value");
```
2. getItem获取value
```
sessionStorage.getItem("key");
```
3. 删除key removeItem 
```
sessionStorage.removeItem("key");
```
4. 清除所有 clear()
```
sessionStorage.clear（）;
```
### 2.vuex 存数据
#### vue的核心是store库管理，store里面的值在任何一个地方改变vue所有用到的组件都会改变，但在使用的时候不能直接赋值改变，需要用commit方法提交。
#### 主要概念
1. state ：存储的数据
2. getters：数据过滤的方法
3. mutations：修改state里面的值。（唯一可以直接改变state的值），使用中通过commit方法调用。
4. actions：改变state的值，他是通过mutations改变state的值的，一般用于异步获取的数据修改，使用中通过dispatch方法调用。
5. modules：数据量大的时候可以用模块化来管理数据。Store和State之间的一层，Store包含多个modules，modules包含state、mutations和actions
#### 使用方式
1. npm install vuex
2. 在项目的src文件夹下面建文件夹store/index.js
3. 在main.js中引入
```
import store from './store/index'
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})
```
4. 编写store/index.js 文件
```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const state = {
  mydata:"第一个数据值"
}
const mutations  = {
  changeMydata(state,msg){
    state.mydata = msg
  }
}
const getters = {
    getMydata(state){
      return state.mydata
    }
}

const actions = {
  getNewMydata(context,text){   //
    context.commit('changeMydata', text)
  }
}

const store = new Vuex.Store({
  modules:{}, //如果有多个模块写在这里。
  state,
  mutations,
  getters,
  actions
})
export default store
````
5. 调用方法
```
this.$store.state.mydata  //直接取值
this.$store.getters.getMydata  //调用getters里面的方法
this.$store.commit('changeMydata',"我是二次更改") //调用mutations里面的方法
this.$store.dispatch("getNewMydata","我是三次更改") //调用actions里面的方法
```

### 3.父像子组件传递数据
```
//父页面中的代码
 <First :fatherdata="forfirst"></First> 
 data () {
    return {
      msg:"",
      forfirst:"来自父页面的值",
      forsecond:"来自父页面的值"
    }
  },
 //子页面的代码
  export default {
    name: "first",
    props:['fatherdata']
  }
```

### 4.子向父传递数据
#### 子组件用$emit方式传递，父组件用v-on方式接受

```
//子页面传递值的时候
created(){
    this.$emit('get-Child', this.childValue)
  }
 //父页面接收值
 <div>{{getsecond}}</div>
 <Second :fatherdata="forsecond" v-on:get-Child="getChild"></Second>
 
  methods:{
    getChild (childValue) {
      this.getsecond = childValue
    }
  }
```
### 5.不同组件之间的数据传递（vue-bus）
```
npm install vue-bus -s

main.js 引入
import bus from 'vue-bus'
Vue.use(bus)

使用方法
this.$bus.emit("eventName",obj)
this.$bus.on("eventName",(data) => {})
this.$bus.off("eventName")
```
### 6.路由携带数据


