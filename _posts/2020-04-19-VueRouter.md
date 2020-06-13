---
   layout: post
   title: "Vue路由"                                                        
   date: 2020-04-19 06:00:00 +0530
   categories: Vue
---
  Vue


# VueRouter

`<router-link to="/login"/>`

`<router-view/>`

## 路由配置及嵌套子路由

```
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    //重定向
    redirect: '/home'
  },
  {
    path: '/home',
    name: "主页",
    component: () => import('../views//home/Home'),
    children: [
      {
        path: '',
        redirect: '/home/homeone',
      },
      {
        path: '/home/homeone',
        name: "主页一",
        component: () => import('../views/home/child/HomeOne'),
      },
      {
        path: '/home/hometwo',
        name: "主页二",
        component: () => import('../views/home/child/HomeTwo'),
      }
    ]
  },
  {
    path: '/about',
    name: "about",
    component: () => import('../views/myApp/About'),
    //独享守卫
  },
  {
    path: '/login',
    name: "login",
    component: () => import('../views/myApp/login/Login'),
  },
 ]
  
  
  
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

## 带参路由及取出参数

### 方式一

#### 路由配置

```
  {
    path: '/detail/:id',
    name: "detail",
    component: () => import('../views/myApp/detail/Detail'),
  },
```

#### 传递参数

```
<router-link :to="`/detail/${id}`"></router-link>
```

#### 参数获取

```
{{$route.params.id}}
```

### 方式二(推荐 没有和router耦合)

#### 路由配置

```
{
    path: '/detail/:id',
    name: "detail",
    component: () => import('../views/myApp/detail/Detail'),
    props:true//加上配置的属性
  },
```

#### 参数传递

```
<router-link :to="`/detail/${id}`"></router-link>
```

#### 参数获取

```
<template>
  <div>
    {{id}}
  </div>
</template>

<script>
  export default {
    props:['id'],//接收
  }
</script
```

## 导航守卫

### 全局导航守卫  (举例)    判断是否登录   非实例

```
//对常量router  
router.beforeEach((to, from, next) => {
  //判断是否登录
  if(to.path === '/about' && !window.isLogin){
    next('/login?redirect='+to.path);
  }else{
    next();
  }
})
```

#### 登录组件

```
<template>
  <div>
   Login
   <button @click="onLigin">登录</button>
  </div>
</template>

<script>
  export default {
    methods: {
      onLigin() {
        window.isLogin = true;
        //获取查询参数
        const redirect = this.$route.query.redirect || '/';
        //路由重定向地址
        this.$router.push(redirect)
      }
    }
  }
</script>

<style scoped>

</style>
```

### 独享守卫

```
{
    path: '/about',
    name: "about",
    beforeEnter(to, from, next) {
      //判断是否登录
      if(!window.isLogin){
        next('/login?redirect='+to.path);
      }else{
        next();
      }
    },
    component: () => import('../views/myApp/About'),
    //独享守卫
  },
```

### 组件内部守卫

### 注意

* `this.$route.query`  当前活动路由参数
* `this.$router.push()`  路由跳转

## 元数据meta

```
var router = new VueRouter({
    routes: [{
        name: 'home', path: '/home', component: HomeComponent
    },
    {
        name: 'customers', path: '/customers', component: CustomerListComponent,
        meta: {
            auth: true
        }

    },
    {
        name: 'detail', path: '/detail/:id', component: CustomerComponent,
        meta: {
            auth: true
        }

    },
    {
        name: 'login', path: '/login', component: LoginComponent
    }
    ]
});
```

```
//注册全局事件钩子
router.beforeEach(function (to, from, next) {
    //如果路由中配置了meta auth信息，则需要判断用户是否登录；
    if (to.matched.some(r => r.meta.auth)) {
        //登录后会把token作为登录的标示，存在localStorage中
        if (!localStorage.getItem('token')) {
            console.log("需要登录");
            next({
                path: '/login',
                query: { to: to.fullPath }
            })
        } else {
            next();
        }
    } else {
        next()
    }
});
```

# VueX

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {//状态
   
  },
  mutations: {//改变状态方法
   
  },
  actions: {//异步操作
   
  },
  modules: {
  }
})

```

## 访问

```
import store from '../store';

store.state.xxx
```

```
全局引入

$store.state.xxx

this.$store.state.xxx
```

## 改变state

```
 this.$store.commit('login');
```

## 异步操作

```
actions: {//异步操作
    requestLogin(context) {
      console.log(context)
      //返回Promise

      return new Promise(resolve => {
        setTimeout(() => {
          context.commit('login')
          resolve(true)
        },1000)
      })
    }
  },

//派发actions  接收到的是Promise
this.$store.dispatch("requestLogin").then(isLogin => {
       //操作
      });
```

## 简化 使用对象解构

```
修改前
<router-link v-if="!$store.state.isLogin" to="/login">登录</router-link>

修改后
<router-link v-if="!$store.state.isLogin" to="/login">登录</router-link>

import {mapSatte} from 'vuex';
  export default {
   computed: {
     ...mapSatte(['isLogin'])
   }	
  }


同样actions
修改前
//派发actions  接收到的是Promise
this.$store.dispatch("requestLogin").then(isLogin => {
       //操作
      });
      
修改后
import {mapActions} from 'vuex';

...mapActions(['requestLogin']),映射出来

this.requestLogin().then(isLogin => {
       //操作
      });
    
    //解构
  import {mapSatte} from 'vuex';  
  import {mapActions} from 'vuex';  
  import {mapGetters} from 'vuex';  
  
  //映射
  ...mapSatte(['isLogin'])
  ...mapActions(['requestLogin'])
  ...mapGetters(['getLogin'])
```

### 传参数

```
 this.requestLogin({username: "tom"}).then(isLogin => {
 
 接收
  requestLogin(context,payload) {
      console.log(context,payload)
```

## 原理

```
vuex
vue数据劫持功能
上下文

vue-router
hashChange
```

# Vue实现原理

## 工作机制

### 初始化

* new Vue() 做了什么
  * 初始化生命周期, props, methods, data, computed watch等
  * 通过Object.defineProperty 设置getter和setter方法, 用来实现`响应式`和`依赖收集`
  * 依赖收集: 那个数据链跟新那个部分
* 挂载之后 ---->首先是编译过程(parse,optimize,generate) ---> 渲染函数(render)(依赖收集)----->虚拟DOM树----->patch()---->生成真实DOM
* 当数据发生变化watch  重新生成虚拟DOM树   比较原来的虚拟DOM(Diff)   哪些地方变,哪些地方重新渲染 高效

* parse  抽象语法树AST   动态生成js代码   
* optimize   标记静态节点
* generate  将第一步生成的AST转化为渲染函数render funcrion   转化为真正的渲染树

#### 简化

```
数据----监听者(用于监听数据变化)----观察者(用于更新数据变化)--管理者(dep 依赖收集 哪里需要变化)-----编译(编译为浏览器认识的代码)------重新渲染
```

### 接口数据模拟

* vue.config.js

```
module.exports = {
  //配置地址BASE_URL
  publicPath: '/kcart',
  configureWebpack: {//配置webpack
    devServer: {
      before(app) {
        //app 是express的实例
        //模拟数据请求  http://localhost:8080/goods
        app.get('/goods',(req,res) => {
          res.json([
            {id:1, text:'abc'},
            {id:2, text:'cde'}
          ])
        })
      }
    }
  }
}
```

# 令牌机制

