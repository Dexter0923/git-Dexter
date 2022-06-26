# 事件监听

在前端开发中，我们需要经常和用于交互。

这个时候，我们就必须监听用户发生的时间，比如点击、拖拽、键盘事件等等

在Vue中如何监听事件呢？使用v-on指令

## v-on介绍

作用：绑定事件监听器

缩写：@

预期：Function | Inline Statement | Object

参数：event

## v-on基础

这里，我们用一个监听按钮的点击事件，来简单看看v-on的使用下面的代码中，我们使用了v-on:click="counter++”

另外，我们可以将事件指向一个在methods中定义的函数

```vue
<div id="app">
	<h2>{{counter}}</h2>
	<button v-on:click="btnClick">+</button>
	<button v-on:click="btn02Click">-</button>
	<!-- <button @click="counter++">按钮1</button>
	<button @click="btnClick">按钮2</button> -->
</div>
```

注：v-on也有对应的语法糖：

v-on:click可以写成@click

```vue
<div id="app">
	<h2>{{counter}}</h2>
	<button @click="btnClick">+</button>
	<button @click="btn02Click">-</button>
</div>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			counter: 0
		},
		methods:{
			btnClick(){
				this.counter++
			},
			btn02Click(){
				this.counter--
			}
		}
	})
</script>
```

## v-on参数

当通过methods中定义方法，以供@click调用时，需要注意参数问题：

情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去

情况二：如果需要同时传入某个参数，同时需要event时，可以通过$event传入事件。

```vue
<div id="app">
	<h2>{{counter}}</h2>
	<button @click="btnClick">+1</button>
	<button @click="btn02Click(10,$event)">+10</button>
</div>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			counter: 0
		},
		methods:{
			btnClick(event){
				console.log(event);
				this.counter++
			},
			btn02Click(count,event){
				console.log(event);
				this.counter +=10;
			}
		}
	})
</script>
```

## v-on修饰符

在某些情况下，我们拿到event的目的可能是进行一些事件处理。

Vue提供了修饰符来帮助我们方便的处理一些事件：

.stop - 调用 event.stopPropagation()。

.prevent - 调用 event.preventDefault()。

.{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调。

.native - 监听组件根元素的原生事件。

.once - 只触发一次回调。

```vue
<div id="app">
	<!-- .stop修饰符的使用 -->
	<div @click="divClick">
		aaaaaaaa
		<button @click.stop="btnClick">按钮</button>
	</div>
	<br>
	<!-- .prevent的使用 -->
	<form action="https://www.baidu.com/" method="">
		<input type="submit" value="提交" @click.prevent="submitClick" >
	</form>
	
	<!-- .监听某个键盘的键帽 -->
	<!-- <input type="text" @keyup="keyUp()"> -->
	<!-- 监听回车的 -->
	<input type="text" @keyup.enter="keyUp()">
</div>
```

## v-if、v-else-if、v-else

v-if、v-else-if、v-else这三个指令与JavaScript的条件语句if、else、else if类似。

Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件

简单的案例演示：

```vue
<div id="app">
	<h2 v-if="score>=90">优秀</h2>
	<h2 v-else-if="score>=80">良好</h2>
	<h2 v-else-if="score>=60">及格</h2>
	<h2 v-else>不合格</h2>
	<h1>{{result}}</h1>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			score:99
		},
		computed:{
			result(){
				let fs = '';
				if(this.score>=90){
					fs = '优秀'
				}else if(this.score>=80){
					fs = '良好'
				}else if(this.score>=60){
					fs = '及格'
				}else{
					fs = '不及格'
				}
					return fs
			}
		}
	})
	
</script>
```

## 条件渲染案例

我们来做一个简单的小案例：用户再登录时，可以切换使用用户账号登录还是邮箱地址登录。

```vue
<div id="app">
	<span v-if="isUser">
		<label for="username">用户账号:</label>
		<input type="text" id="username" placeholder="用户账号">
	</span>
	<span v-else="isUser">
		<label for="email">用户邮箱:</label>
		<input type="text"  id="email"  placeholder="用户邮箱"/>
		</span>
	<button @click="isUser = !isUser">切换类型</button>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			isUser:true
		}
	})
	
</script>
```

### 案例小问题

#### 小问题：

如果我们在有输入内容的情况下，切换了类型，我们会发现文字依然显示之前的输入的内容。

但是按道理讲，我们应该切换到另外一个input元素中了。

在另一个input元素中，我们并没有输入内容。为什么会出现这个问题呢？

#### 问题解答：

这是因为Vue在进行DOM渲染时，出于性能考虑，会尽可能的复用已经存在的元素，而不是重新创建新的元素。

在上面的案例中，Vue内部会发现原来的input元素不再使用，直接作为else中的input来使用了。

#### 解决方案：

如果我们不希望Vue出现类似重复利用的问题，可以给对应的input添加key并且我们需要保证key的不同

```vue
<div id="app">
	<span v-if="isUser">
		<label for="username">用户账号:</label>
		<input type="text" id="username" placeholder="用户账号" key="username">
	</span>
	<span v-else="isUser">
		<label for="email">用户邮箱:</label>
		<input type="text"  id="email"  placeholder="用户邮箱" key="email"/>
		</span>
	<button @click="isUser = !isUser">切换类型</button>
</div>
```

## v-show

v-show的用法和v-if非常相似，也用于决定一个元素是否渲染：

v-if和v-show对比v-if和v-show都可以决定一个元素是否渲染，那么开发中我们如何选择呢？

v-if当条件为false时，压根不会有对应的元素在DOM中。

v-show当条件为false时，仅仅是将元素的display属性设置为none而已。

开发中如何选择呢？

当需要在显示与隐藏之间切片很频繁时，使用v-show

当只有一次切换时，通过使用v-if

## v-for遍历数组

当我们有一组数据需要进行渲染时，我们就可以使用v-for来完成。

v-for的语法类似于JavaScript中的for循环。

格式如下：item in items的形式。

我们来看一个简单的案例：

如果在遍历的过程中不需要使用索引值

v-for="movie in movies"

依次从movies中取出movie，并且在元素的内容中，我们可以使用Mustache语法，来使用movie

```vue
<div id="app">
			<ul>
				<li v-for="item in movies">{{item}}</li>
				<br>
				<li v-for="(movie,index) in movies">{{index+1}}.{{movie}}</li>
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
```



如果在遍历的过程中，我们需要拿到元素在数组中的索引值呢？

语法格式：v-for=(item, index) in items

其中的index就代表了取出的item在原数组的索引值。

## v-for遍历对象

v-for可以用户遍历对象：

比如某个对象中存储着你的个人信息，我们希望以列表的形式显示出来。

```vue
<div id="app">
	<ul>
		<!-- 1.在遍历对象的过程中，如果只是获取一个值，那么获取到的是value -->
		<li v-for="item in info">{{item}}</li>
	</ul>
	
	<ul>
		<!--2.获取key和value 格式:(value，key) -->
		<li v-for="(value,key) in info">{{value}}-{{key}}</li>
	</ul>
	
	<ul>
		<li v-for="(value,key,index) in info">{{index+1}}.{{value}}-{{key}}</li>
	</ul>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			info:{
				name:'Dexrer',
				age:18,
				height:1.88
			}
		}
	})
	
</script>
```

## 组件的key属性

官方推荐我们在使用v-for时，给对应的元素或组件添加上一个:key属性。

为什么需要这个key属性呢（了解）？

这个其实和Vue的虚拟DOM的Diff算法有关系。

这里我们借用React’s diff algorithm中的一张图来简单说明一下：

当某一层有很多相同的节点时，也就是列表节点时，

我们希望插入一个新的节点我们希望可以在B和C之间加一个F，Diff算法默认执行起来是这样的。即把C更新成F，D更新成C，E更新成D，最后再插入E，是不是很没有效率？

所以我们需要使用key来给每个节点做一个唯一标识

Diff算法就可以正确的识别此节点找到正确的位置区插入新的节点。

所以一句话，key的作用主要是为了高效的更新虚拟DOM。

