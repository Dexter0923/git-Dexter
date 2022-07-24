## Vue CLI2详解

<a href="https://imgtu.com/i/j7IXxs"><img src="https://s1.ax1x.com/2022/07/19/j7IXxs.png" alt="j7IXxs.png" border="0" /></a>

## Runtime-Compiler和Runtime-only的区别

```javascript
//main.js
<script>
    //runtime-compiler(v1) 第一种里面的 main.js
   	new Vue({
  		components: { App },
   		template: '<App/>'
	})
	//runtime-only(v2) 	  第二种里面的 main.js
    new Vue({
  		el: '#app',
  		render: h =>h (App)
	})
    //runtime-compiler(v1)
	//template ->ast->render -> vdom -> UI
	//runtime-only(v2)(1.性能更高2.下面的代码量更少)
	//render -> vdom -> UI
	//那么.vue文件中的template是由谁处理的了?
	//是由vue-template-compiler  .vue文件封装处理
</script>
```

## npm run build

<a href="https://imgtu.com/i/j7oZs1"><img src="https://s1.ax1x.com/2022/07/19/j7oZs1.png" alt="j7oZs1.png" border="0" /></a>

## npm run dev

<a href="https://imgtu.com/i/j7o1RH"><img src="https://s1.ax1x.com/2022/07/19/j7o1RH.png" alt="j7o1RH.png" border="0" /></a>

## 箭头函数

箭头函数也是一种定义函数的方式

```javascript
<script>
    //箭头函数
    const aaa = (参数列表) =>{
        
    }
    //箭头函数中的this的使用
    //什么时候使用箭头函数
    //之前
    setTimeout(function () {
        console.log(this);
    },1000)
	//现在
	setTimeout(() => {
        console.log(this);
    },1000)
	
	//问题:箭头函数中的this是如何查找的了?
	//答案:向外层作用域中，一层层查找this，直到有this的定义.
	const obj ={
        aaa(){
            setTimeout(function(){
                console.log(this);//window
            })
            
            setTimeout(() => {
        		console.log(this);//obj对象
        	}) 
        }
    }
    obj.aaa()
</script>
```

## vue-router

**什么是路由？**

路由是一个网络工程里面的术语。

路由（routing）就是通过互联的网络把信息从源地址传输到目的地址的活动. --- 维基百科

**路由器提供了两种机制: 路由和转送.**

路由是决定数据包从来源到目的地的路径.

转送将输入端的数据转移到合适的输出端.

**路由中有一个非常重要的概念叫路由表.**

路由表本质上就是一个映射表, 决定了数据包的指向.

### 后端路由阶段

**早期的网站开发整个HTML页面是由服务器来渲染的.**

服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示.

但是, 一个网站, 这么多页面服务器如何处理呢?

一个页面有自己对应的网址, 也就是URL.

URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理.

Controller进行各种处理, 最终生成HTML或者数据, 返回给前端.

这就完成了一个IO操作.

**上面的这种操作, 就是后端路由.**

当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户顿.

这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化.

**后端路由的缺点:**

一种情况是整个页面的模块由后端人员来编写和维护的.

另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码.

而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情.

### 前端路由阶段

**前后端分离阶段：**

随着Ajax的出现, 有了前后端分离的开发模式.

后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中.

这样做最大的优点就是前后端责任的清晰, 后端专注于数据上, 前端专注于交互和可视化上.

并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可.

目前很多的网站依然采用这种模式开发.

**单页面富应用阶段:**

其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由.

也就是前端来维护一套路由规则.

**前端路由的核心是什么呢？**

改变URL，但是页面不进行整体的刷新。

### 前端路由的规则

#### URL的hash

URL的hash也就是锚点(#), 本质上是改变window.location的href属性.

我们可以通过直接赋值location.hash来改变href, 但是页面不发生刷新

```javascript
location.hash = '/Dexter'
```

#### HTML5的history模式：pushState

history接口是HTML5新增的, 它有五种模式改变URL而不刷新页面.

```javascript
history.pushState({},'','/Dexter')
```

#### HTML5的history模式：replaceState

```javascript
history.replaceState({},'','/Dexter') //这个浏览器不能选择后退
```

#### HTML5的history模式：go

```javascript
history.go(1)
history.go(-1)
//history.back() 等价于 history.go(-1) //浏览器后退
//history.forward() 则等价于 history.go(1) //浏览器前进
//这三个接口等同于浏览器界面的前进后退。
```

## 认识vue-router

vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。

我们可以访问其官方网站对其进行学习: https://router.vuejs.org/zh/

**vue-router是基于路由和组件的**

路由用于设定访问路径, 将路径和组件映射起来.

在vue-router的单页面应用中, 页面的路径的改变就是组件的切换.

## 安装vue-router

```javascript
npm install vue-router --save

npm i vue-router@3.2.0
```

## 使用vue-router

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

const Home = () => import('../views/home/Home')
const Category = () => import('../views/category/Category')
const Cart = () => import('../views/cart/Cart')
const Profile = () => import('../views/profile/Profile')

// 1.安装插件
Vue.use(VueRouter)

// 2.创建VueRouter对象  路由对象
const routes = [
  //配置映射关系
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    component: Home
  },
  {
    path: '/category',
    component: Category
  },
  {
    path: '/cart',
    component: Cart
  },
  {
    path: '/profile',
    component: Profile
  }
]
const router = new VueRouter({
  //配置路由和组件之间的应用关系
  routes,
  //修改模式 网址里的#消除了
  mode: 'history'
})

// 3.导出router
export default router
```

### router-link补充

```javascript
<router-link to='/home' tag='li'></router-link>
//属性: to, 用于指定跳转的路径.
//tag: tag可以指定<router-link>之后渲染成什么组件, 比如上面的代码会被渲染成一个<li>元素, 而不是<a>
//replace: replace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中
//active-class: 当<router-link>对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称.
//在进行高亮显示的导航菜单或者底部tabbar时, 会使用到该类.
//但是通常不会修改类的属性, 会直接使用默认的router-link-active即可. 
//该class具体的名称也可以通过router实例的属性进行修改 
//linkActiveClass:'Dexter'
```

### 路由代码跳转

<a href="https://imgtu.com/i/jbEKOA"><img src="https://s1.ax1x.com/2022/07/20/jbEKOA.png" alt="jbEKOA.png" border="0" /></a>

### 动态路由

在某些情况下，一个页面的path路径可能是不确定的，比如我们进入用户界面时，希望是如下的路径：

/user/aaaa或/user/bbbb

除了有前面的/user之外，后面还跟上了用户的ID

这种path和Component的匹配关系，我们称之为动态路由(也是路由传递数据的一种方式)。

```javascript
//1.创建组件
//User.js 
<div>
    <h2>{{$route.params.abc}}</h2>
</div>
//2.配置映射关系 
//index.js
//路由懒加载
const User = () => import('../user/User')
{
    path:'/Dexter/:abc',
    component:User
}
//3.首页显示
//App.js
<router-link to='/user'>用户</router-link>
```

### 认识路由的懒加载

**官方给出了解释:**

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。

如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了

**路由懒加载做了什么?**

路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块.

只有在这个路由被访问到的时候, 才加载对应的组件

### 路由懒加载的效果

<a href="https://imgtu.com/i/jj8imn"><img src="https://s1.ax1x.com/2022/07/24/jj8imn.png" alt="jj8imn.png" border="0" /></a>

### 懒加载的方式

方式一: 结合Vue的异步组件和Webpack的代码分析.

```javascript
const Home = resolve => { require.ensure(['../components/Home.vue'], () => { resolve(require('../components/Home.vue')) })};
```

方式二: AMD写法

```javascript
const About = resolve => require(['../components/About.vue'], resolve);
```

方式三: 在ES6中, 我们可以有更加简单的写法来组织Vue异步组件和Webpack的代码分割

```javascript
const Home = () => import('../components/Home.vue')
```

### 认识嵌套路由

**嵌套路由是一个很常见的功能**

比如在home页面中, 我们希望通过/home/news和/home/message访问一些内容.

一个路径映射一个组件, 访问这两个路径也会分别渲染两个组件.

**实现嵌套路由有两个步骤:**

创建对应的子组件, 并且在路由映射中配置对应的子路由.

在组件内部使用<router-view>标签.

```javascript
//index.js
const Message = () => import('../views/message/Message')
{
    path: '/home',
    component: Home,
    children:[
      {
       path:'message',
       component:Message
      }
    ],
 },
```

### 传递参数的方式

传递参数主要有两种类型: params和query

**params的类型:**

配置路由格式: /router/:id

传递的方式: 在path后面跟上对应的值

传递后形成的路径: /router/123, /router/abc

**query的类型:**

配置路由格式: /router, 也就是普通配置

传递的方式: 对象中使用query的key作为传递方式

传递后形成的路径: /router?id=123, /router?id=abc

### 传递参数方式一: < router-link>

```javascript
//App.vue
<router-link
      :to="{
        path:'/profile' + 123,
        query:{name:'Dexter',age:18}
      }"
 >档案</router-link>
```

### 传递参数方式二: JavaScript代码

```javascript
//App.vue
export default {
  name: 'App',
  methods:{
    toProfile(){
      this.$router.push({
        path:'/profile' + 123,
        query:{name:'Dexter',age:18}
      })
    }
  },
}
```

### 导航守卫

**为什么使用导航守卫?**

**我们来考虑一个需求: 在一个SPA应用中, 如何改变网页的标题呢?**

网页标题是通过<title>来显示的, 但是SPA只有一个固定的HTML, 切换不同的页面时, 标题并不会改变.

但是我们可以通过JavaScript来修改<title>的内容.window.document.title = '新的标题'.

那么在Vue项目中, 在哪里修改? 什么时候修改比较合适呢?

**普通的修改方式:**

我们比较容易想到的修改标题的位置是每一个路由对应的组件.vue文件中.

通过mounted声明周期函数, 执行对应的代码进行修改即可.

但是当页面比较多时, 这种方式不容易维护(因为需要在多个页面执行类似的代码).

有没有更好的办法呢? 使用导航守卫即可.

**什么是导航守卫?**

vue-router提供的导航守卫主要用来监听监听路由的进入和离开的.

vue-router提供了beforeEach和afterEach的钩子函数, 它们会在路由即将改变前和改变后触发.

### 导航守卫使用

我们可以利用beforeEach来完成标题的修改.

首先, 我们可以在钩子当中定义一些标题, 可以利用meta来定义

其次, 利用导航守卫,修改我们的标题.

```javascript
const routes = [
  //配置映射关系
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    component: Home,
    meta:{
      title:'首页'
    }
  },
  {
    path: '/category',
    component: Category,
    meta:{
      title:'分类'
    }
  },
  {
    path: '/cart',
    component: Cart,
    meta:{
      title:'购物车'
    }
  },
  {
    path: '/profile',
    component: Profile,
    meta:{
      title:'个人'
    }
  }
]
//导航钩子的三个参数解析:
//to: 即将要进入的目标的路由对象.
//from: 当前导航即将要离开的路由对象.
//next: 调用该方法后, 才能进入下一个钩子.
router.beforeEach((to, from, next) => {
  document.title = to.matched[0].meta.title
  next()
})
```

### keep-alive遇见vue-router

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

**它们有两个非常重要的属性:**

include - 字符串或正则表达，只有匹配的组件会被缓存

exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存

router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存：

```javascript
<keep-alive   exclude="组件中的name" include="组件中的name">
      <router-view>
            //所有路径匹配到的视图组件都会被缓存
      </router-view>
 </keep-alive>
//通过create声明周期函数来验证
```

## 什么是Promise呢？

Promise到底是做什么的呢？

Promise是异步编程的一种解决方案。

## Promise基本使用

```JavaScript
<script>
    //过去的处理方式
    setTimeout(function () {
        let data = 'Hello World'
        console.log(content)
    },1000)
	
	//现在使用Promise
    new Promise((resolve, reject) => {
        setTimeout(function () {
            resolve('Dexter')
            reject('彭于晏')
        }).then(data => {
            console.log(data);
        }).catch(error => {
            console.log(error);
        })
        },1000)
//     new Promise很明显是创建一个Promise对象
//     小括号中((resolve, reject) => {})也很明显就是一个函数，而且我们这里用的是之前刚刚学习过的箭头函数。
// 但是resolve, reject它们是什么呢？
// 我们先知道一个事实：在创建Promise时，传入的这个箭头函数是固定的（一般我们都会这样写）
// resolve和reject它们两个也是函数，通常情况下，我们会根据请求数据的成功和失败来决定调用哪一个。
// 成功还是失败？
// 如果是成功的，那么通常我们会调用resolve(message)，这个时候，我们后续的then会被回调。
// 如果是失败的，那么通常我们会调用reject(error)，这个时候，我们后续的catch会被回调
</script>
```

## Promise三种状态

pending：等待状态，比如正在进行网络请求，或者定时器没有到时间。

fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()

reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

## Promise链式调用

```javascript
<script>
	new Promise((resolve, reject) => {
        setTimeout(function () {
           resolve('Dexter')
        }).then(data => {
            console.log(data); // => Dexter
            return Promise.resolve(data + '快')
        }).then(data => {
            console.log(data);  // => Dexter快
            return Promise.resolve(data + '点')
        }).then(data => {
            console.log(data);  // => Dexter快点
            return Promise.resolve(data + '跑')
        }).then(data => {
            console.log(data);  // => Dexter快点跑
            return Promise.reject(data + '！')
        }).then(data => {
            console.log(data);  //这里没有输出,这部分代码不会执行
            return Promise.reject(data + '啊')
        }).catch(data => {
            console.log(data);  // => Dexter快点跑啊
        })
        },1000)
</script>
```

## Vuex是做什么的?

官方解释：Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。

它采用 集中式存储管理 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

**状态管理到底是什么？**

状态管理模式、集中式存储管理这些名词听起来就非常高大上，让人捉摸不透。

其实，你可以简单的将其看成把需要多个组件共享的变量全部存储在一个对象里面。

然后，将这个对象放在顶层的Vue实例中，让其他组件可以使用。

那么，多个组件是不是就可以共享这个对象中的所有变量属性了呢？

等等，如果是这样的话，为什么官方还要专门出一个插件Vuex呢？难道我们不能自己封装一个对象来管理吗？

当然可以，只是我们要先想想VueJS带给我们最大的便利是什么呢？

没错，就是响应式。

如果你自己封装实现一个对象能不能保证它里面所有的属性做到响应式呢？

当然也可以，只是自己封装可能稍微麻烦一些。

不用怀疑，Vuex就是为了提供这样一个在多个组件间共享状态的插件，用它就可以了。

**管理什么状态呢?**

如果你做过大型开放，你一定遇到过多个状态，在多个界面间的共享问题。

比如用户的登录状态、用户名称、头像、地理位置信息等等。

比如商品的收藏、购物车中的物品等等。这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且它们还是响应式的

## Vuex状态管理图例

<a href="https://imgtu.com/i/jjdYy8"><img src="https://s1.ax1x.com/2022/07/24/jjdYy8.png" alt="jjdYy8.png" border="0" /></a>

## Vuex简单使用

**安装Vuex**

```javascript
npm install Vuex --save
```

**简单的案例**

```javascript
//store/index.js
import Vuex from 'vuex'
import  Vue from  'vue'

Vue.use(Vuex)

const store = new Vuex.Store({
  state:{
    count:0
  },
  mutations:{
    increment(state){
      state.count++
    },
    decrement(state){
      state.count--
    }
  }
})

export default  store

//main.js
import store from "./store";

/* eslint-disable no-new */
new Vue({
  el: '#app',
  //挂载路由
  router:router,
  store,
  render: h =>h (App)
})

//App.vue
<template>
  <div id="app">
    <p>{{count}}</p>
    <button @click="addition">+1</button>
    <button @click="subtraction">-1</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  computed:{
    count:function () {
        return this.$store.state.count
    }
  },
  methods:{
    addition(state) {
      this.$store.commit('increment')
    },
    subtraction(state) {
      this.$store.commit('decrement')
    }
  }
}
//我们来对使用步骤，做一个简单的小节：
//1.提取出一个公共的store对象，用于保存在多个组件中共享的状态
//2.将store对象放置在new Vue对象中，这样可以保证在所有的组件中都可以使用到
//3.在其他组件中使用store对象中保存的状态即可
//通过this.$store.state.属性的方式来访问状态
//通过this.$store.commit('mutation中方法')来修改状态
//注意事项：
//我们通过提交mutation的方式，而非直接改变store.state.count。
//这是因为Vuex可以更明确的追踪状态的变化，所以不要直接改变store.state.count的值。
```

## Vuex核心概念

- State
- Getters
- Mutation
- Action
- Module

### State单一状态树

Vuex提出使用单一状态树, 什么是单一状态树呢？

英文名称是Single Source of Truth，也可以翻译成单一数据源。

单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便的管理和维护。

### Getters基本使用

```javascript
//store/index.js
//获取学生年龄大于20的个数。
const store = new Vuex.Store({
  state:{
    students:[
      {id:100,name:'Dexter', age: 18},
      {id:101,name:'彭于晏', age: 36},
      {id:101,name:'张艺兴', age: 30},
      {id:101,name:'迪丽热巴', age: 26},
    ]
  },
    //Getters作为参数和传递参数
  getters:{
    mo20:state => {
      return state.students.filter(s => s.age >20)
    },
    mo20abc:(state,getters) =>{
      return getters.mo20.length
    }
  },
})

//App.vue
<h2>{{$store.getters.mo20}}</h2>
<h2>{{$store.getters.mo20abc}}</h2>
```

### Mutation状态更新

Vuex的store状态的更新唯一方式：**提交MutationMutation**

**主要包括两部分：**

字符串的事件类型（type）

一个回调函数（handler）,该回调函数的第一个参数就是state。

**mutation的定义方式：**

```javascript
mutations:{
    increment(state){
      state.count++
    },
  }
```

**通过mutation更新**

```javascript
increment:function () {
      this.$store.commit('increment')
    }
```

### Mutation传递参数

在通过mutation更新数据的时候, 有可能我们希望携带一些额外的参数

参数被称为是mutation的载荷(Payload)

Mutation中的代码

```javascript
//store/index.js
state:{
    count:0,
  },
mutations:{
    increment(state,count){
      state.count += count
    },
  },

//App.vue
   methods:{
   addCount(count){
      //1.普通的提交封装
     this.$store.commit('increment',count)
   }
  },
 //
  <h2>{{$store.state.count}}</h2>
  <button @click="addCount(5)">+5</button>
```

### Mutation响应规则

Vuex的store中的state是响应式的, 当state中的数据发生改变时, Vue组件会自动更新.

这就要求我们必须遵守一些Vuex对应的规则:

提前在store中初始化好所需的属性.

当给state中的对象添加新属性时, 使用下面的方式:

方式一: 使用Vue.set(obj, 'newProp', 123)

方式二: 用心对象给旧对象重新赋值
