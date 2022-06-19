# 什么是计算属性？

我们知道，在模板中可以直接通过插值语法显示一些data中的数据。

但是在某些情况，我们可能需要对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示

比如我们有firstName和lastName两个变量，我们需要显示完整的名称。

但是如果多个地方都需要显示完整的名称，我们就需要写多个{{firstName}} {{lastName}}

我们可以将上面的代码换成计算属性：

我们发现计算属性是写在实例的computed选项中的。

```vue
<div id="app">
	{{fullName}}
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			firtName: 'Dexter',
			lastName:'彭于晏',
		},
		computed:{
			fullName: function(){
				return this.firtName + '' + this.lastName
			}
		}
	})
</script>
```

## 计算属性的复杂操作

```vue
<div id="app">
	书籍总价格:{{totalPrice}}
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			books:[
				{id:100, name:'C++' ,price:153},
				{id:101, name:'Java' ,price:253},
				{id:102, name:'Python' ,price:143},
				{id:103, name:'Vue' ,price:135},
			]
		},
		computed:{
			totalPrice:function(){
				let total = 0;
				for(let i=0;i<this.books.length;i++){
					total += this.books[i].price
				}
				return total
			}
		}
	})
</script>
```

## 计算属性的setter和getter

每个计算属性都包含一个getter和一个setter在上面的例子中，我们只是使用getter来读取。

在某些情况下，你也可以提供一个setter方法（不常用）。

在需要写setter的时候，代码如下：

```vue
<div id="app">
	{{fullName}}
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			firstName: 'Dexter',
			lastName: '彭于晏',
		},
		computed: {
			fullName: {
				set: function(newValue) {
					console.log('--------调用了fullName的set');
					const names = newValue.split('');
					this.firstName = names[0];
					this.lastName = names[1];
				},
				get: function() {
					console.log('--------调用了fullName的get');
					return this.firstName + '' + this.lastName
				}
			}
		}
	})
</script>
```

## 计算属性的缓存

我们可能会考虑这样的一个问题：

methods和computed看起来都可以实现我们的功能，

那么为什么还要多一个计算属性这个东西呢？

原因：计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。

# ES6补充

## let/var

事实上var的设计可以看成JavaScript语言设计上的错误. 但是这种错误多半不能修复和移除, 以为需要向后兼容.

大概十年前, Brendan Eich就决定修复这个问题, 于是他添加了一个新的关键字: let.

我们可以将let看成更完美的var

### 块级作用域

JS中使用var来声明一个变量时, 变量的作用域主要是和函数的定义有关.

针对于其他块定义来说是没有作用域的，比如if/for等，这在我们开发中往往会引起一些问题。

ES5之前因为if和for都没有块级作用域的概念，所以在很多时候，我们都必须借助于function的作用解决应用外面变量的问题

为什么闭包可以解决问题: 函数是一个作用域.

ES6中,加入了let,let它是有if和for的块级作用域.

```vue
//监听按钮的点击
var btns = document.getElementsByTagName('button');
	for(var i=0;i<btns.length;i++){
		(function(i){
			btns[i].onclick = function(){
				alert('点击了'+ i +'个')
			}
		})(i)
	}
```

```vue
const btns = document.getElementsByTagName('button');
	for(let i=0;i < btns.length;i++){
			btns[i].onclick = function(){
				alert('点击了'+ i +'个')
			}
	}
```

## const的使用

### const关键字

在很多语言中已经存在, 比如C/C++中, 主要的作用是将某个变量修饰为常量.

在JavaScript中也是如此, 使用const修饰的标识符为常量, 不可以再次赋值.

### 什么时候使用const呢?

当我们修饰的标识符不会被再次赋值时, 就可以使用const来保证数据的安全性.

建议: 在ES6开发中,优先使用const, 只有需要改变某一个标识符的时候才使用let. 

### const的注意

const注意一:

一旦给const修饰的标识符被赋值之后,不能修改

```
const a = 20;
a = 30;//错误 不可以修改
```

const注意二:

在使用const定义标识符,必须进行赋值

```
const name;
```

const注意三:

常量的含义是指向的对象修改,但是可以改变对象内部的属性

```vue
	const obj ={
		name: 'Dexter',
		age: 18,
		height:1000
	}
	console.log(obj);
	obj.name = '彭于晏',
	obj.age = 20,
	obj.height = 1231;
	console.log(obj);
```

## 对象增强写法

ES6中，对对象字面量进行了很多增强。

属性初始化简写和方法的简写：

```vue
// 1.属性的增强写法
  const name = 'why';
  const age = 18;
  const height = 1.88

  // ES5的写法
  const obj = {
    name: name,
    age: age,
    height: height
  }
	//ES6的写法
  const obj = {
    name,
    age,
    height,
  }
  console.log(obj);
```

```vue
// 2.函数的增强写法
  //ES5的写法
  const obj = {
    run: function () {
  
    },
    eat: function () {
  
    }
  }
	//ES6的写法
  const obj = {
    run() {

    },
    eat() {

    }
  }
```

