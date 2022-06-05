# Vue中的MVVM

## MVVM是什么：

MVVM是 Model-View-ViewModel 的缩写，即 模型-视图-视图模型。

Model：数据模型，后端传递的数据。(data，props，computed等部分）
View：代表 UI 组件，它负责将数据模型转化成 UI 展现出来。（template部分）
ViewModel：是一个同步View 和 Model的对象。MVVM模式的核心，它是连接Model和View的桥梁。
 vue的核心，双向绑定、监听（watch）、操作（methods）等部分

<a href="https://imgtu.com/i/XdFAfI"><img src="https://s1.ax1x.com/2022/06/04/XdFAfI.png" alt="XdFAfI.png" border="0" /></a>

# 创建Vue实例传入的options

你会发现，我们在创建Vue实例的时候，传入了一个对象options。
这个options中可以包含哪些选项呢？
详细解析： https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE
目前掌握这些选项：
el: 
类型：string | HTMLElement
作用：决定之后Vue实例会管理哪一个DOM。
data: 
类型：Object | Function （组件当中data必须是一个函数）
作用：Vue实例对应的数据对象。
methods: 
类型：{ [key: string]: Function }
作用：定义属于Vue的一些方法，可以在其他地方调用，也可以在指令中使用。

# 插值操作

## Mustache

如何将data中的文本数据，插入到HTML中呢？
我们已经学习过了，可以通过Mustache语法(也就是双大括号)。
Mustache: 胡子/胡须.
我们可以像下面这样来使用，并且数据是响应式的

```vue
<div id="app">
	<h2>Hello {{message}}</h2>//插入标签中
	<h2>{{firstName}} {{lastName}}</h2>//使用了俩个Mustache
	<h2>{{counter}}</h2>//也可以是一个表达式
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			firstName: '你好啊',
			lastName: '彭于晏',
			counter:100
		}
	})
</script>
```

## v-once

但是，在某些情况下，我们可能不希望界面随意的跟随改变
这个时候，我们就可以使用一个Vue的指令
v-once: 
该指令后面不需要跟任何表达式(比如之前的v-for后面是由跟表达式的)
该指令表示元素和组件(组件后面才会学习)只渲染一次，不会随着数据的改变而改变。

```vue
<div id="app">
	<h2 v-once>{{message}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
		}
	})
</script>
浏览器开发者模式的console可以用
app.message='abc'
界面不会跟随改变
```

## v-html

某些情况下，我们从服务器请求到的数据本身就是一个HTML代码
	如果我们直接通过{{}}来输出，会将HTML代码也一起输出。
	但是我们可能希望的是按照HTML格式进行解析，并且显示对应的内容。
如果我们希望解析出HTML展示
可以使用v-html指令
	该指令后面往往会跟上一个string类型
	会将string的html解析出来并且进行渲染

```vue
<div id="app">
	<h2>{{link}}</h2>
	<h2 v-html="link"></h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			link:'<a href="https://www.baidu.com/">百度一下</a>'
		}
	})
</script>
浏览器界面显示:
<a href="https://www.baidu.com/">百度一下</a>
百度一下
```

## v-text

v-text作用和Mustache比较相似：都是用于将数据显示在界面中
v-text通常情况下，接受一个string类型

```vue
<div id="app">
    <h2>{{message}}</h2>
    </h2 v-text="message"></h2>
</div>
```

## v-pre

v-pre用于跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法。
比如下面的代码：
第一个h2元素中的内容会被编译解析出来对应的内容
第二个h2元素中会直接显示{{message}}

```vue
<div id="app">
    <h2>{{message}}</h2>
	<h2 v-pre>{{message}}</h2>
</div>
```

## v-cloak

在某些情况下，我们浏览器可能会直接显然出未编译的Mustache标签。
cloak: 斗篷

```vue
		<style type="text/css">
			[v-cloak] {
				display: none;
			}
		</style>
	</head>
	<body>
<div id="app" v-cloak>
		{{name}}
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	// 在vue解析之前，div中有一个v-clock
	// 在vue解析之后，div中没有一个属性v-clock
	setTimeout(function () {
		const app = new Vue({
			el: '#app',
			data:{
				name:'Dexter'
			}
		})
	},1000)
</script>
```

