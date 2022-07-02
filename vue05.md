## 检测数组更新

因为Vue是响应式的，所以当数据发生变化时，Vue会自动检测数据变化，视图会发生对应的更新。

Vue中包含了一组观察数组编译的方法，使用它们改变数组也会触发视图的更新。

```vue
<div id="app">
	<ul>
		<li v-for="item in items">{{item}}</li>
	</ul>
	<button @click="an">按钮</button>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			items: ['a','b','d','c']
		},
		methods:{
			an(){
				// push方法 给数组后面添加元素
				// this.items.push('aa','BB');
				
				// 2.pop方法 删除数组中的最后一个元素
				// this.items.pop();
				
				// 3.shift方法 删除数组中的第一个元素
				// this.items.shift();
				
				// 4.unshift方法 给数组前面添加元素
				// this.items.unshift('aa','bb');
				
				//5.splice方法  作用:删除元素/插入元素/替换元素
				// 删除元素:第二个参数传入你要删除几个元素(如果没有传,就删除后面所有的元素)
				// 替换元素:第二个参数，表示我们要替换几个元素，后面是用于替换前面的元素
				// 插入元素:第二个参数，传入0，并且后面跟上要插入的元素
				// this.items.splice(1,2)
				// this.items.splice(1,3,'e','f','g')
				
				// 6.sort方法 排列数据
				// this.items.sort()
				
				// 7.reverse方法 反转数据
				// this.items.reverse()
				
				// 不会修改
				// this.items[0] = 'L'
				
				// 通过下面的方法
				this.items.splice(0,1,'L')
				Vue.set(this.items,0,'L')
			}
		}
	})
	
</script>
```

## 案例实现

```vue
<style type="text/css">
			.active{
				color: green;
			}
		</style>
<div id="app">
	<ul>
		<li v-for="(item,index) in movies"
		:class="{active:currentIndex === index}"
		@click="liClick(index)"
		>
		{{index+1}}.{{item}}
		</li>
	</ul>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			movies:['肖申克的救赎','阿甘正传','绿里奇迹','少年派的奇幻漂流'],
			currentIndex:0
		},
		methods:{
			liClick(index){
				this.currentIndex = index
			}
		}
	})
	
</script>
```

## 购物车案例

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<style type="text/css">
		table{
			border: 1px solid #e9e9e9;
			border-collapse: collapse;
			border-spacing: 0;
		}
		th{
			background-color: #f7f7f7;
			color: #008000;
			font-weight: 600;
		}
		th,td{
			padding: 8px 16px;
			border: 1px solid #E9E9E9;
			text-align: left;
		}
	</style>
	<body>
		<div id="app">
			<div v-if="books.length">
			<table>
				<thead>
					<tr>
						<th></th>
						<th>书籍名称</th>
						<th>出版日期</th>
						<th>价格</th>
						<th>购买数量</th>
						<th>操作</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(item,index) in books">
						<td>{{item.id}}</td>
						<td>{{item.name}}</td>
						<td>{{item.date}}</td>
						<td>{{item.price | showPrice}}</td>
						<td>
							<button @click="decrement(index)" v-bind:disabled="item.conut <=1">-</button>
							{{item.conut}}
							<button @click="increment(index)">+</button>
						</td>
						<td><button @click="removeHandle(index)">移除</button></td>
					</tr>
				</tbody>
			</table>
			<h2>总价格:{{totalPrice | showPrice}}</h2>
			</div>
			<h2 v-else>购物车为空</h2>
		</div>
		<script type="text/javascript" src="./js/vue.js"></script>
		<script type="text/javascript">
			const app = new Vue({
				el: '#app',
				data:{
					books:[
						{
							id:1,
							name:'《肖申克的救赎》',
							date: '1994-1',
							price:85.00,
							conut:1
						},
						{
							id:2,
							name:'《阿甘正传》',
							date: '1994-2',
							price:105.00,
							conut:1
						},
						{
							id:3,
							name:'《绿里奇迹》',
							date: '1999-3',
							price:95.00,
							conut:1
						},
						{
							id:4,
							name:'《少年派的奇幻漂流》',
							date: '2012-4',
							price:155.00,
							conut:1
						},
					]
				},
				methods:{
					increment(index){
						this.books[index].conut++;
					},
					decrement(index){
						this.books[index].conut--;
					},
					removeHandle(index){
						this.books.splice(index,1);
					}
				},
				filters:{
					showPrice(price){
						return '￥' + price.toFixed(2)
					}
				},
				computed:{
					totalPrice() {
						// 第一种方法
						// let totalPrice = 0
						// for(let i=0; i < this.books.length; i++ ){
						// 	totalPrice += this.books[i].price * this.books[i].conut
						// }
						// return totalPrice
						
						// 第二种方法 
						// let totalPrice = 0
						// for( let i in this.books){
						// 	const book =this.books[i]
						// 	totalPrice += book.price * book.conut
						// }
						// return totalPrice
						
						// 第三种方法
						// let totalPrice =0
						// for( let item of this.books){
						// 	totalPrice += item.price * item.conut
						// }
						// return totalPrice
						
						// 第四种写法 JavaScript 高阶函数的写法
						return this.books.reduce(function (preValue,book){
							return preValue + book.price * book.conut
						},0)
					}
				}
			})
		</script>
	</body>
</html>
```

## JavaScript 高阶函数的使用

```vue
<script>
	// 1.需求: 取出所有小于100的数字
	// 2.需求:将所有小于100的数字进行转化: 全部*2
	// 3.需求:将所有得到的数字相加,得到最终的结果
	// 编程范式: 命令式编程/声明式编程
	// 编程范式: 面向对象编程(第一公民:对象)/函数式编程(第一公民:函数)
	// filter/map/reduce
	// filter中的回调函数有一个要求: 必须返回一个boolean值
	// true: 当返回true时, 函数内部会自动将这次回调的n加入到新的数组中
	// false: 当返回false时, 函数内部会过滤掉这次的n
	const nums = [10,20,111,82,96,110,16,84,19,80,99]
	// 第一种方法
	let total = nums.filter(function(n){
		return n < 100
	}).map(function(n){
		return n * 2
	}).reduce(function(preValue,n){
		return preValue + n
	},0)
	console.log(total)
	
	// 第二种方法
	//  let total = nums.filter(n => n < 100).map(n => n * 2).reduce((pre, n) => pre + n);
	// console.log(total)
</script>
```

## 表单绑定v-model

表单控件在实际开发中是非常常见的。特别是对于用户信息的提交，需要大量的表单。

Vue中使用v-model指令来实现表单元素和数据的双向绑定。

案例的解析：

当我们在输入框输入内容时

因为input中的v-model绑定了message，所以会实时将输入的内容传递给message，message发生改变。

当message发生改变时，因为上面我们使用Mustache语法，将message的值插入到DOM中，所以DOM会发生响应的改变。

所以，通过v-model实现了双向的绑定。

```vue
<div id="app">
	<input type="text" :value="message" @input="valueChage">
	<input type="text" :value="message" @input="message = $event.target.value">
	<input type="text" v-model="message" />
	 <!-- 当然，我们也可以将v-model用于textarea元素 -->
	 <textarea :value="message"  @input="message = $event.target.value"></textarea>
	 <textarea v-model="message"></textarea>
	<h2>{{message}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		methods:{
			valueChage(event){
				this.message = event.target.value
			}
		}
	})
	
</script>
```

## v-model原理

v-model其实是一个语法糖，它的背后本质上是包含两个操作：

1.v-bind绑定一个value属性

2.v-on指令给当前元素绑定input事件也就是说下面的代码：

等同于下面的代码：

```vue
<input type="text" v-model="message">
等同于
<input type="text" v-bind:value="message" v-on:input="message = $event.target.value">
```

## v-model：radio 单选框

```vue
<div id="app">
	<label for="male">
		<input type="radio" id="male"  value="男" v-model="sex">男
	</label>
	<label for="female">
		<input type="radio" id="female"  value="女" v-model="sex">女
	</label>
	<h2>你选择的是:{{sex}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			sex:'男'
		}
	})
	
</script>
```

## v-model：checkbox 复选框

复选框分为两种情况：

单个勾选框和多个勾选框

单个勾选框：

v-model即为布尔值。

此时input的value并不影响v-model的值。

多个复选框：

当是多个复选框时，因为可以选中多个，所以对应的data中属性是一个数组。

当选中某一个时，就会将input的value添加到数组中。

```vue
<div id="app">
	<label for="agreement">
		<!-- agreement 协议 -->
		<input type="checkbox" id="agreement" v-model="agreement">同意协议
	</label>
	<h2>你选择的是:{{agreement}}</h2>
	<button type="button" :disabled="! agreement">下一步</button>
	
	<label>
		<!-- hobbies:业余爱好 -->
		<input type="checkbox"  value="篮球"  v-model="hobbies"/>篮球
		<input type="checkbox"  value="羽毛球" v-model="hobbies"/>羽毛球
		<input type="checkbox"  value="足球" v-model="hobbies"/>足球
		<input type="checkbox"  value="棒球" v-model="hobbies"/>棒球
	</label>
	<h2>你选择的是:{{hobbies}}</h2>
	
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			agreement:false,
			hobbies: []
		}
	})
	
</script>
```

## v-model：select   下拉列表框

和checkbox一样，select也分单选和多选两种情况。

单选：只能选中一个值。

v-model绑定的是一个值。

当我们选中option中的一个时，会将它对应的value赋值到mySelect中

多选：可以选中多个值。

v-model绑定的是一个数组。

当选中多个值时，就会将选中的option对应的value添加到数组mySelects中

```vue
<div id="app">
	<select v-model="mySelect">
		<option value ="苹果">苹果</option>
		<option value="橘子">橘子</option>
		<option value="香蕉">香蕉</option>
		<option value="西瓜">西瓜</option>
	</select>
	<h2>我选择的是:{{mySelect}}</h2>
	
	<select v-model="mySelect2" multiple="multiple">
		<option value ="苹果">苹果</option>
		<option value="橘子">橘子</option>
		<option value="香蕉">香蕉</option>
		<option value="西瓜">西瓜</option>
	</select>
	<h2>我选择的是:{{mySelect2}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			mySelect:'西瓜',
			mySelect2:[]
		}
	})
	
</script>
```

## 值绑定

就是动态的给value赋值而已：

我们前面的value中的值，可以回头去看一下，都是在定义input的时候直接给定的。

但是真实开发中，这些input的值可能是从网络获取或定义在data中的。

所以我们可以通过v-bind:value动态的给value绑定值。



```vue
<div id="app">
	<label v-for="item in hobbies2" :for="item">
		<input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
	</label>
    <h2>你选择的是:{{hobbies}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			hobbies: [],
			hobbies2:['篮球','足球','棒球','台球','高尔夫球','乒乓球']
		}
	})
</script>
```

## 修饰符

### lazy修饰符：

默认情况下，v-model默认是在input事件中同步输入框的数据的。

也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变。

lazy修饰符可以让数据在失去焦点或者回车时才会更新

### number修饰符：

默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理。

但是如果我们希望处理的是数字类型，那么最好直接将内容当做数字处理。

number修饰符可以让在输入框中输入的内容自动转成数字类型

### trim修饰符：

如果输入的内容首尾有很多空格，通常我们希望将其去除trim修饰符可以过滤内容左右两边的空格

```vue
<div id="app">
	<!-- 第一种 lazy修饰符 -->
	<label>
		<input type="text" v-model.lazy="message">
	</label>
	<h2>当前内容:{{message}}</h2>
	
	<!-- 第二种 number修饰符 -->
	<label>
		<input type="number" v-model.number="age">
	</label>
	<h2>当前内容:{{age}}</h2>
	
	<!-- 第三种修饰符 trim修饰符-->
	<label>
		<input type="text" v-model.trim="Space">
	</label>
	<h2>当前内容:{{Space}}</h2>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter',
			age:'',
			Space:'彭于晏'
		}
	})
	
</script>
```

