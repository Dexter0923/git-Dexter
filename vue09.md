### Action的基本定义

**我们强调, 不要再Mutation中进行异步操作.**

但是某些情况, 我们确实希望在Vuex中进行一些异步操作, 比如网络请求, 必然是异步的. 这个时候怎么处理呢Action类似于Mutation, 但是是用来代替Mutation进行异步操作的.

**Action的基本使用代码如下:**

```javascript
const store = new Vuex.Store({
  state:{
    count:0,
  mutations:{
    increment(state){
      state.count ++
    }
  },
  actions:{
    increment(context) {
      context.commit('increment')
    }
  },
})
```

**context是什么?**

context是和store对象具有相同方法和属性的对象.

也就是说, 我们可以通过context去进行commit相关的操作, 也可以获取context.state等

但是注意, 这里它们并不是同一个对象, 为什么呢? 我们后面学习Modules的时候, 再具体说.

**这样的代码是否多此一举呢?**

我们定义了actions, 然后又在actions中去进行commit, 这样不是多此一举吗

事实上并不是这样, 如果在Vuex中有异步操作, 那么我们就可以在actions中完成了.

### Action的分发

在Vue组件中, 如果我们调用action中的方法, 那么就需要使用dispatch

```javascript
methods:{
    increment() {
        this.$store.dispatch('increment')
    }
}


```

同样的, 也是支持传递payload

```javascript
//App.vue
methods:{
    increment() {
        this.$store.dispatch('increment',{cCount:5})
    }
}

//store/index.js
const store = new Vuex.Store({
  state:{
    count:0,
  mutations:{
    increment(state,payload){
      state.count += payload.cCount
    }
  },
  actions:{
    increment(context,payload) {
      Dexter(() => {
          context.commit('increment',payload)
      },1000)
    }
  }
})

```

### Action返回的Promise

前面我们学习ES6语法的时候说过, Promise经常用于异步操作.

在Action中, 我们可以将异步操作放在一个Promise中, 并且在成功或者失败后, 调用对应的resolve或reject.

```javascript
//store/index.js 
actions:{
    increment(context) {
      return new Promise((resolve) => {
          Dexter(() => {
          context.commit('increment')
          resolve('11111')
     	},1000)
      })
    }
  }
//App.vue
methods:{
    increment() {
        this.$store.dispatch('increment').then(res => {
            console.log('完成了更新操作');
            console.log(res);///11111
        })
    }
}
```

### 认识Module

odule是模块的意思, 为什么在Vuex中我们要使用模块呢?

Vue使用单一状态树,那么也意味着很多状态都会交给Vuex来管理.

当应用变得非常复杂时,store对象就有可能变得相当臃肿.

为了解决这个问题, Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutations、actions、getters等

```javascript
const  moduleA ={
  state:{
      name:Dexter
  },
  mutations:{},
  actions:{},
  getters:{}
}

const  moduleB ={
  state:{
      age:18,
  },
  mutations:{},
  actions:{},
  getters:{}
}

const store = new Vuex.Store({
  modules:{
    a: moduleA,
    b: moduleB
  }
})

<h2>{{$store.state.a.name}}</h2> // moduleA的状态
<h2>{{$store.state.b.age}}</h2> // moduleB的状态
```

### Module局部状态

上面的代码中, 我们已经有了整体的组织结构, 下面我们来看看具体的局部模块中的代码如何书写.

我们在moduleA中添加state、mutations、getters

mutation和getters接收的第一个参数是局部状态对象

<a href="https://imgtu.com/i/vpiXm8"><img src="https://s1.ax1x.com/2022/07/27/vpiXm8.png" alt="vpiXm8.png" border="0" /></a>

<a href="https://imgtu.com/i/vpFpfs"><img src="https://s1.ax1x.com/2022/07/27/vpFpfs.png" alt="vpFpfs.png" border="0" /></a>

**注意:**

虽然, 我们的doubleCount和increment都是定义在对象内部的.

但是在调用的时候, 依然是通过this.$store来直接调用的.

### Actions的写法

actions的写法呢?接收一个context参数对象

局部状态通过 context.state 暴露出来，根节点状态则为 context.rootState

<a href="https://imgtu.com/i/vpFmtJ"><img src="https://s1.ax1x.com/2022/07/27/vpFmtJ.png" alt="vpFmtJ.png" border="0"></a>

如果getters中也需要使用全局的状态, 可以接受更多的参数

<a href="https://imgtu.com/i/vpFek4"><img src="https://s1.ax1x.com/2022/07/27/vpFek4.png" alt="vpFek4.png" border="0"></a>

### 项目结构

当我们的Vuex帮助我们管理过多的内容时, 好的项目结构可以让我们的代码更加清晰.

<a href="https://imgtu.com/i/vpFV7F"><img src="https://s1.ax1x.com/2022/07/27/vpFV7F.png" alt="vpFV7F.png" border="0"></a>

