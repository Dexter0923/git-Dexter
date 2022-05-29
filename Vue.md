

# 什么是Vue.js？

Vue是一个渐进式的框架，什么是渐进式的呢?

- 渐进式意味着你可以将Vue作为你应用的一部分嵌入其中，带来更丰富的交互体验。
- 或者如果你希望将更多的业务逻辑使用Vue实现，那么Vue的核心库以及其生态系统。
- 比如Core+Vue-router+Vuex，也可以满足你各种各样的需求。

Vue有很多特点和Web开发中常见的高级功能

- 解耦视图和数据
- 可复用的组件
- 前端路由技术
- 状态管理
- 虚拟DOM

学习Vue.js的前提?

从零学习Vue开发，并不需要你具备其他类似于Angular、React，甚至是jQuery的经验。

但是你需要具备一定的HTML  CSS  JavaScript基础

# Vue测试

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue 测试实例</title>
		<script type="text/javascript" src="./js/vue.js"></script>
	</head>
	<body>
		<div id="app">
			<h1>{{message}}</h1>
			<h2>{{name}}</h2>
		</div>

		<script>
		 const app =	new Vue({
				el: '#app',
				data: {
					message: '你好，彭于晏!',
					name:'Dexter'
				}
			})
		</script>
		
	</body>
</html>
创建Vue对象的时候，传入了一些options : {}
{}中包含了el属性:该属性决定了这个Vue对象挂载到哪一个元素上，很明显，我们这里是挂载到了id为app的元素上
{}中包含了data属性:该属性中通常会存储一些数据
√这些数据可以是我们直接定义出来的，比如像上面这样。
√也可能是来自网络，从服务器加载的。
```

# Vue列表显示

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue 测试实例</title>
		<script type="text/javascript" src="./js/vue.js"></script>
	</head>
	<body>
		<div id="app">
			<ul>
				<li v-for="item in movies">{{item}}</li>
			</ul>
		</div>

		<script>
		 const app = new Vue({
				el: '#app',
				data: {
					//movies 电影
					movies:['肖申克的救赎','阿甘正传','图灵']
				}
			})
		</script>
		
	</body>
</html>
浏览器开发者模式的console可以用
app.movies.push('少年派的奇幻漂流')
来添加

```

# Vue计数器

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Vue 测试实例</title>
		<script type="text/javascript" src="./js/vue.js"></script>
	</head>
	<body>
		<div id="app">
			<h1>当前计数：{{counter}}</h1>
			<button v-on:click="add()"></button>
			<button v-on:click="sub()"></button>
		</div>

		<script>
		 const app = new Vue({
				el: '#app',
				data: {
					counter:0
				},
				methods:{
					add:function(){
						console.log('add被执行');
						this.counter++
					},
					sub:function(){
						console.log('sub被执行');
						this.counter--
					}
				}
			})
		</script>
		
	</body>
</html>
这里，我们又要使用新的指令和属性了
新的属性: methods，该属性用于在Vue对象中定义方法。
新的指令:@click,该指令用于监听某个元素的点击事件，并且需要指定当发生点击时，执行的方法(方法通常是methods中定义的方法)
```

