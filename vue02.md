# 绑定属性

## v-bind

但是，除了内容需要动态来决定外，某些属性我们也希望动态来绑定。
比如动态绑定a元素的href属性
比如动态绑定img元素的src属性
这个时候，我们可以使用v-bind指令：
作用：动态绑定属性
缩写：:
预期：any (with argument) | Object (without argument)
参数：attrOrProp (optional)

```vue
<div id="app">
	<a v-bind:href="url">百度一下</a>
	<img v-bind:src="img" >
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			url:'https://www.baidu.com/',
		img:'https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png'
		}
	})
</script>
```

## v-bind语法糖

```vue
v-bind有一个对应的语法糖，也就是简写方式
<div id="app">
	<a :href="url">百度一下</a>
	<img :src="img" >
</div>
```

## v-bind绑定class(一)

很多时候，我们希望动态的来切换class，比如：
当数据为某个状态时，字体显示红色。
当数据另一个状态时，字体显示黑色。
绑定class有两种方式：
对象语法
数组语法

```vue
<style type="text/css">
			.active{
				color: red;
			}
		</style>
	</head>
	<body>
<div id="app">
  	//<h2 v-bind:class="{类名1:布尔值,类名2:布尔值}">{{message}}</h2>
	<h2 v-bind:class="{active:isActive ,line:isLine}">{{message}}</h2>
	<button v-on:click="btnClick">按钮</button>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message:'Dexter',
			isActive: true,
			isLine: true
		},
		methods:{
			btnClick:function(){
				this.isActive = !this.isActive
			}
		}
	})
</script>
```

## v-bind绑定class(二)

绑定方式：对象语法
对象语法的含义是:class后面跟的是一个对象。
对象语法有下面这些用法：

```vue
用法一：直接通过{}绑定一个类
<h2 :class="{active: isActive}">Hello World</h2>

用法二：也可以通过判断，传入多个值
<h2 :class="{active: isActive, line: isLine}">Hello World</h2>

用法三：和普通的类同时存在，并不冲突
注：如果isActive和isLine都为true，那么会有title/active/line三个类
<h2 class="title" :class="{active: isActive, line: isLine}">Hello World</h2>

用法四：如果过于复杂，可以放在一个methods或者computed中
注：classes是一个计算属性
<h2 v-bind:class="classes()">{{message}}</h2>
	methods:{
			btnClick:function(){
				this.isActive = !this.isActive
			},
			classes:function(){
				return {active:this.isActive ,line:this.isLine}
			}
		}
```

## v-bind绑定class(三)

绑定方式：数组语法
数组语法的含义是:class后面跟的是一个数组。
数组语法有下面这些用法：

```vue
用法一：直接通过{}绑定一个类
<h2 :class="[active]">Hello World</h2>

用法二：也可以传入多个值
<h2 :class=“[active, line]">Hello World</h2>

用法三：和普通的类同时存在，并不冲突
注：会有title/active/line三个类
<h2 class="title" :class=“[active, line]">Hello World</h2>

用法四：如果过于复杂，可以放在一个methods或者computed中
注：classes是一个计算属性
<h2 class="title" :class="classes">Hello World</h2>



<div id="app">
	<h2 :class="[active,line]">{{message}}</h2>
	<h2 :class="classes()">{{message}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			active:'aaaaa',
			line:'bbbbb'
		},
		methods:{
			classes:function(){
				return [this.active,this.line]
			}
		}
	})
</script>
```

## v-bind绑定style(一)

我们可以利用v-bind:style来绑定一些CSS内联样式。
在写CSS属性名的时候，比如font-size
我们可以使用驼峰式 (camelCase)  fontSize 
或短横线分隔 (kebab-case，记得用单引号括起来) ‘font-size’
绑定class有两种方式：
对象语法
数组语法

## v-bind绑定style(二)

```vue
绑定方式一：对象语法
:style="{color: currentColor, fontSize: fontSize + 'px'}"
style后面跟的是一个对象类型
对象的key是CSS属性名称
对象的value是具体赋的值，值可以来自于data中的属性

绑定方式二：数组语法
<div v-bind:style="[baseStyles, overridingStyles]"></div>
style后面跟的是一个数组类型
多个值以，分割即可
```

